@ControllerAdvice
public class CustomSoapExceptionHandler extends ResponseEntityExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    protected ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex,
                                                                  HttpHeaders headers,
                                                                  HttpStatus status,
                                                                  WebRequest request) {
        // Handle validation errors and construct an appropriate SOAP fault response
        // For example, you can create a custom SOAP fault object and return it
        SoapFault fault = createCustomSoapFault(ex);
        return handleExceptionInternal(ex, fault, headers, HttpStatus.BAD_REQUEST, request);
    }

    // Define other exception handlers for specific exceptions related to HTTP 400 errors

    // ...

    // Define a fallback handler for generic exceptions
    @ExceptionHandler(Exception.class)
    protected ResponseEntity<Object> handleGenericException(Exception ex, WebRequest request) {
        // Handle any generic exceptions that are not explicitly handled
        // Construct an appropriate SOAP fault response for generic exceptions
        SoapFault fault = createCustomSoapFault(ex);
        return handleExceptionInternal(ex, fault, new HttpHeaders(), HttpStatus.BAD_REQUEST, request);
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
