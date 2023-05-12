# app1

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import java.util.Collections;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Map;

public class CustomHttpServletRequestWrapper extends HttpServletRequestWrapper {

    private final Map<String, String> headers;

    public CustomHttpServletRequestWrapper(HttpServletRequest request) {
        super(request);
        this.headers = new HashMap<>();
    }

    public void addHeader(String name, String value) {
        headers.put(name, value);
    }

    @Override
    public String getHeader(String name) {
        String headerValue = super.getHeader(name);
        if (headerValue == null) {
            headerValue = headers.get(name);
        }
        return headerValue;
    }

    @Override
    public Enumeration<String> getHeaderNames() {
        Enumeration<String> headerNames = super.getHeaderNames();
        if (headerNames == null) {
            headerNames = Collections.enumeration(headers.keySet());
        }
        return headerNames;
    }

    @Override
    public Enumeration<String> getHeaders(String name) {
        Enumeration<String> headerValues = super.getHeaders(name);
        if (headerValues == null) {
            String value = headers.get(name);
            if (value != null) {
                headerValues = Collections.enumeration(Collections.singletonList(value));
            }
        }
        return headerValues;
    }
}
