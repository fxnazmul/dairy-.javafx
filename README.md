# dairy-.javafx
start code
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;
import java.io.*;

public class SimpleDiaryFX extends Application {
    private static final String PASSWORD = "123";
    private static final String FILE = "diary.txt";

    @Override
    public void start(Stage stage) {
        PasswordField pass = new PasswordField();
        Button login = new Button("Login");
        TextArea diary = new TextArea();
        Button save = new Button("Save Entry");
        Button view = new Button("View Entries");
        Button delete = new Button("Delete All Entries");
        TextArea output = new TextArea();
        output.setEditable(false);
        Label msg = new Label();

        VBox loginBox = new VBox(10, pass, login, msg);
        VBox diaryBox = new VBox(10, diary, save, view, delete, output);
        Scene loginScene = new Scene(loginBox, 300, 150);
        Scene diaryScene = new Scene(diaryBox, 400, 400);

        login.setOnAction(e -> {
            if (pass.getText().equals(PASSWORD)) {
                stage.setScene(diaryScene);
            } else {
                msg.setText("Wrong password!");
            }
        });

        save.setOnAction(e -> {
            try (BufferedWriter w = new BufferedWriter(new FileWriter(FILE, true))) {
                w.write(diary.getText() + "\n");
                diary.clear();
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        });

        view.setOnAction(e -> {
            output.clear();
            try (BufferedReader r = new BufferedReader(new FileReader(FILE))) {
                String line;
                while ((line = r.readLine()) != null) {
                    output.appendText(line + "\n");
                }
            } catch (IOException ex) {
                output.setText("No entries found.");
            }
        });

        delete.setOnAction(e -> {
            try (BufferedWriter w = new BufferedWriter(new FileWriter(FILE))) {
                w.write(""); // Clear file contents
                output.clear();
                output.setText("All entries deleted.");
            } catch (IOException ex) {
                output.setText("Error deleting entries.");
            }
        });

        stage.setTitle("Digital Diary");
        stage.setScene(loginScene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
