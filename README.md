public class CustomEndpointInterceptor implements EndpointInterceptor {

    @Override
    public boolean handleRequest(MessageContext messageContext, Object endpoint) throws Exception {
        // Handle the request
        // ...

        return true;
    }

    @Override
    public boolean handleResponse(MessageContext messageContext, Object endpoint) throws Exception {
        // Handle the response
        // ...

        return true;
    }

    @Override
    public boolean handleFault(MessageContext messageContext, Object endpoint) throws Exception {
        // Handle the fault
        // ...

        return true;
    }

    @Override
    public void afterCompletion(MessageContext messageContext, Object endpoint, Exception ex) throws Exception {
        // Log the HTTP 400 errors
        if (ex instanceof SoapFaultException && HttpStatus.BAD_REQUEST.value() == getStatusCode((SoapFaultException) ex)) {
            System.out.println("HTTP 400 error occurred: " + ex.getMessage());
        }
    }

    private int getStatusCode(SoapFaultException ex) {
        SoapFault fault = ex.getFault();
        if (fault instanceof Soap12Fault) {
            return ((Soap12Fault) fault).getFaultCode().getValue();
        } else {
            return fault.getFaultCode();
        }
    }
}
====================================

    @Bean
    public CustomEndpointInterceptor endpointInterceptor() {
        return new CustomEndpointInterceptor();
    }

    @Bean
    public SaajSoapMessageFactory messageFactory() {
        return new SaajSoapMessageFactory();
    }
======================================

public class CustomMessageDispatcherServlet extends AbstractSoapMessageDispatcherServlet {

    @Override
    protected void doService(HttpServletRequest request, HttpServletResponse response) throws Exception {
        if (request.getContentLength() == 0) {
            response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Empty SOAP request");
            return;
        }

        super.doService(request, response);
    }
}
