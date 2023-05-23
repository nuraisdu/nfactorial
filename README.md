# nfactorial
nfactorial game
import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.stage.Stage;

public class Main extends Application {
 private static final int WIDTH = 640;
    private static final int HEIGHT = 480;
    private static final int RADIUS = 100;
    private static final double MAX_DISTANCE = 10.0;
    private static final double MIN_SPEED = 100.0; 
    private static final Color PERFECT_COLOR = Color.GREEN;
    private static final Color ERROR_COLOR = Color.RED;
private Circle circle;
    private double lastX;
    private double lastY;
    private double totalDistance;
    private double totalTime;
    private boolean isDrawing;

    @Override
    public void start(Stage primaryStage) {
        circle = new Circle();
        circle.setRadius(RADIUS);
        circle.setCenterX(WIDTH / 2);
        circle.setCenterY(HEIGHT / 2);
        circle.setFill(PERFECT_COLOR);
        Group root = new Group(circle);
        Scene scene = new Scene(root, WIDTH, HEIGHT, Color.BLACK);
        scene.setOnMousePressed(event -> {
          
            lastX = event.getX();
            lastY = event.getY();
            totalDistance = 0.0;
            totalTime = 0.0;
            isDrawing = true;
        });

        scene.setOnMouseDragged(event -> {
            if (isDrawing) {
       double currentX = event.getX();
  double currentY = event.getY();
      double distance = Math.hypot(currentX - lastX, currentY - lastY);
                totalDistance += distance;
                double timeDiff = 1.0 / 60.0; 
                totalTime += timeDiff;
                if (distance < MAX_DISTANCE) {
                    lastX = currentX;
                    lastY = currentY;
                } else {circle.setFill(ERROR_COLOR);
                    isDrawing = false;
                    System.out.println("Distance error: Too close for point");
                }
    double speed = distance / timeDiff;
      if (speed < MIN_SPEED) {
             circle.setFill(ERROR_COLOR);
           isDrawing = false;      System.out.println("Speed error: Drawing too slow!");
                }
 double perfection = 100.0 - (totalDistance / (2 * Math.PI * RADIUS)) * 100.0;
                System.out.printf("Circle perfection: %.2f%%%n", perfection);
       lastX = currentX;
                lastY = currentY;
            }
        });
 primaryStage.setScene(scene);
        primaryStage.setTitle("Draw Perfect Circle");
        primaryStage.show();
    }public static void main(String[] args) {
        launch(args);
    }
}
