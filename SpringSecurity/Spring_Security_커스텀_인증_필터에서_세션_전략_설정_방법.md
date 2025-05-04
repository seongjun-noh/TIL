# Spring Security 세션 전략 설정

## 개요
Spring Security에서 기본 폼 로그인을 사용하는 경우, `SecurityConfig`에서 `SessionManagement`만 설정해도 세션 관리가 자동으로 적용됩니다. 
즉, 별도의 세션 전략 설정 없이도 기본적인 세션 관리가 가능합니다.

그러나 `AbstractAuthenticationProcessingFilter`를 커스텀하여 로그인 필터를 구현하는 경우, 세션 전략(`SessionStrategy`)은 자동으로 설정되지 않으므로, 이를 별도로 명시적으로 설정해주어야 합니다.

## 구현 방법

### SecurityConfig
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.authentication.session.ConcurrentSessionControlAuthenticationStrategy;
import org.springframework.security.web.authentication.session.SessionFixationProtectionStrategy;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public CustomAuthenticationFilter customAuthenticationFilter() {
        // 세션 전략 설정 (예: 최대 세션 1개)
        SessionAuthenticationStrategy sessionStrategy = new ConcurrentSessionControlAuthenticationStrategy(maximumSessions());
        
        // CustomAuthenticationFilter에 세션 전략 주입
        return new CustomAuthenticationFilter("/login", sessionStrategy);
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login")
                .permitAll()
                .and()
            .addFilter(customAuthenticationFilter())
            .sessionManagement()
                .maximumSessions(1) // 기본 세션 관리 설정
                .and()
            .sessionFixation().migrateSession(); // 세션 고정 보호 전략
    }

    @Bean
    public int maximumSessions() {
        return 1; // 사용자당 허용되는 최대 세션 수
    }
}
```

### CustomAuthenticationFilter
```java
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter;
import org.springframework.security.web.authentication.session.SessionAuthenticationStrategy;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class CustomAuthenticationFilter extends AbstractAuthenticationProcessingFilter {

    private final SessionAuthenticationStrategy sessionStrategy;

    public CustomAuthenticationFilter(String defaultFilterProcessesUrl, SessionAuthenticationStrategy sessionStrategy) {
        super(new AntPathRequestMatcher(defaultFilterProcessesUrl));
        this.sessionStrategy = sessionStrategy; // 세션 전략 설정
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
            throws AuthenticationException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        // 사용자 정의 인증 로직 (Authentication 객체 생성)
        CustomAuthenticationToken authRequest = new CustomAuthenticationToken(username, password);

        // 세션 전략 적용
        sessionStrategy.onAuthentication(authRequest, request, response);

        return this.getAuthenticationManager().authenticate(authRequest);
    }
}
```