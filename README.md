# Order Lifecycle Notification System with WebSocket

## Introduction

This project is a **real-time notification system** for tracking order status using **Spring Boot** and **WebSockets**. It bridges the gap between administrators updating order statuses and users tracking them, ensuring seamless, instant communication. 

---

## About the Project

### What does it do?
1. **Admin View**: Enables administrators to update order statuses (Prepared, Ready, Delivered).
2. **User View**: Displays order progress in real time with dynamic UI updates.

![User View - Order Tracking](images/user_view_order_tracking.png)


### Technologies and Concepts:
- **WebSockets** for real-time, bi-directional communication.
- **Spring Boot** for backend services and WebSocket endpoints.
- **Bootstrap** and **JavaScript** for responsive and interactive UI.
- **JSON Messaging** for data exchange.

---

## Real-Time Scenario

### Use Case: Food Delivery
- **Without Real-Time Updates**: Customers refresh pages or call for updates.
- **With This Project**: Instant notifications enhance customer experience, making order tracking effortless.

  ## Dependencies

This project uses the following key dependencies:

- **Spring Boot**: Framework for building the application.
- **Spring WebSocket**: To implement real-time communication.
- **Lombok**: To reduce boilerplate code, especially for data objects like `OrderUpdate`.
- **Bootstrap**: For creating responsive and elegant UI.
- **STOMP**: For message-oriented communication over WebSockets.

These dependencies allow us to create an interactive and dynamic web application for real-time order tracking.

---

## Key Components

### WebSocket Configuration

```java
@Configuration
@EnableWebSocket
public class WebSocketConfig implements WebSocketConfigurer {

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        registry.addHandler(new WebSocketHandler(), "/ws")
                .setAllowedOrigins("*");  // Allowing connections from all origins for testing
    }
}
```

### WebSocket Configuration

#### Explanation:
- **WebSocketConfig**: This class configures the WebSocket handler for real-time communication.
- **`@EnableWebSocket`**: Enables WebSocket support in the application.
- **`registerWebSocketHandlers()`**: Maps the `/ws` endpoint for WebSocket connections, where clients will connect for real-time updates.

### Order Notification Controller

```java
@Controller
public class OrderNotificationController {

    @MessageMapping("/order/status")  // Endpoint to receive updates
    @SendTo("/topic/order")  // Broadcast updates to all subscribers
    public OrderUpdate sendOrderUpdate(OrderUpdate orderUpdate) {
        // Simulate order status update processing
        return orderUpdate;
    }
}
```
#### Explanation:
- **OrderNotificationController**: Handles incoming messages from the client (Admin).
- **`@MessageMapping("/order/status")`**: Listens for incoming messages from clients updating the order status.
- **`@SendTo("/topic/order")`**: Once an update is received, it is broadcasted to all clients subscribed to `/topic/order`, ensuring the users (Client2) see the status changes in real time.

### Flow of the Application

1. **`order.html`** (Client1) enters an Order ID and clicks a status button (e.g., "Prepared").
2. The **OrderUpdate** object, which contains the `orderId` and `status`, is sent to the WebSocket endpoint (`/app/order/status`).
3. The **OrderNotificationController** receives the message and processes it.
4. The controller then broadcasts the updated order status to all subscribers (here **`order-user-client.html`** which is Client2).
5. **`order-user-client.html`** (Client2), who is subscribed to `/topic/order`, receives the order update and sees it reflected in real-time on the tracking page.

This flow enables the admin to update the order status, and users will immediately see these changes without needing to refresh the page, ensuring a seamless experience.

