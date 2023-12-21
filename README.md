import javax.jms.Connection;
import javax.jms.ConnectionFactory;
import javax.jms.Destination;
import javax.jms.JMSException;
import javax.jms.MessageProducer;
import javax.jms.Session;
import javax.jms.TextMessage;
import org.apache.activemq.ActiveMQConnectionFactory;

public class SimpleJmsSender {

    public static void main(String[] args) {
        // 连接工厂 - 特定于提供者
        ConnectionFactory factory = new ActiveMQConnectionFactory("tcp://localhost:61616");

        Connection connection = null;
        try {
            // 创建连接
            connection = factory.createConnection();
            connection.start();

            // 创建会话
            Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

            // 创建目的地（队列或主题）
            Destination destination = session.createQueue("testQueue");

            // 创建消息生产者
            MessageProducer producer = session.createProducer(destination);

            // 创建一条文本消息
            TextMessage message = session.createTextMessage("Hello JMS!");

            // 发送消息
            producer.send(message);

            System.out.println("Message sent: " + message.getText());
        } catch (JMSException e) {
            e.printStackTrace();
        } finally {
            // 关闭连接
            if (connection != null) {
                try {
                    connection.close();
                } catch (JMSException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
<dependencies>
    <!-- ActiveMQ -->
    <dependency>
        <groupId>org.apache.activemq</groupId>
        <artifactId>activemq-all</artifactId>
        <version>5.x.x</version> <!-- 使用适当的版本号 -->
    </dependency>
</dependencies>
