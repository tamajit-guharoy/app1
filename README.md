import org.apache.cxf.message.Message;
import org.apache.cxf.phase.AbstractPhaseInterceptor;
import org.apache.cxf.phase.Phase;

public class IgnoreSoapActionInterceptor extends AbstractPhaseInterceptor<Message> {

    public IgnoreSoapActionInterceptor() {
        super(Phase.PRE_PROTOCOL);
    }

    @Override
    public void handleMessage(Message message) {
        // Remove or ignore the SOAPAction header
        message.getHeaders().remove("SOAPAction");
    }
}
