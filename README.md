@Bean
public EndpointExceptionResolver customEndpointExceptionResolver() {
    return new CustomEndpointExceptionResolver();
}

public class CustomEndpointExceptionResolver implements EndpointExceptionResolver {

    @Override
    public boolean resolveException(MessageContext messageContext, Object endpoint, Exception ex) {
        if (ex instanceof MethodArgumentNotValidException) {
            // Handle validation errors and construct an appropriate SOAP fault response
            // For example, you can create a custom SOAP fault object and return it
            SoapFault fault = createCustomSoapFault(ex);
            messageContext.getResponse().setPayload(fault);
            messageContext.getResponse().setFault(true);
            messageContext.getResponse().setException(ex);
            return true;
        }
        
        // Handle other exceptions and return false to allow them to be processed by other resolvers
        return false;
    }
    
    // Helper method to create a custom SOAP fault object based on the exception
    private SoapFault createCustomSoapFault(Exception ex) {
        // Create and configure the SOAP fault object
        SoapFault fault = new SoapFault();
        fault.setMessage(ex.getMessage());
        // Set other properties of the fault as needed
        return fault;
    }
}
