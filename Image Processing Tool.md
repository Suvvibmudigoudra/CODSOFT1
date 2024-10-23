#include <QApplication>
#include <QMainWindow>
#include <QPushButton>
#include <QVBoxLayout>
#include <QLabel>
#include <QFileDialog>
#include <QImage>
#include <QPixmap>
#include <opencv2/opencv.hpp>

class ImageProcessor : public QMainWindow {
    Q_OBJECT

public:
    ImageProcessor(QWidget *parent = nullptr) : QMainWindow(parent) {
        setupUI();
    }

private slots:
    void loadImage() {
        QString fileName = QFileDialog::getOpenFileName(this, "Open Image", "", "Images (*.png *.xpm *.jpg)");
        if (!fileName.isEmpty()) {
            image = cv::imread(fileName.toStdString());
            displayImage();
        }
    }

    void applyGrayscale() {
        if (!image.empty()) {
            cv::cvtColor(image, image, cv::COLOR_BGR2GRAY);
            displayImage();
        }
    }

    void applyBlur() {
        if (!image.empty()) {
            cv::blur(image, image, cv::Size(5, 5));
            displayImage();
        }
    }

    void adjustBrightness(int value) {
        if (!image.empty()) {
            image.convertTo(image, -1, 1, value); // Adjust brightness
            displayImage();
        }
    }

    void saveImage() {
        QString fileName = QFileDialog::getSaveFileName(this, "Save Image", "", "Images (*.png *.xpm *.jpg)");
        if (!fileName.isEmpty()) {
            cv::imwrite(fileName.toStdString(), image);
        }
    }

private:
    cv::Mat image;
    QLabel *imageLabel;

    void setupUI() {
        QWidget *centralWidget = new QWidget(this);
        QVBoxLayout *layout = new QVBoxLayout(centralWidget);
        imageLabel = new QLabel(this);
        
        QPushButton *loadButton = new QPushButton("Load Image", this);
        QPushButton *grayButton = new QPushButton("Apply Grayscale", this);
        QPushButton *blurButton = new QPushButton("Apply Blur", this);
        QPushButton *saveButton = new QPushButton("Save Image", this);
        
        layout->addWidget(loadButton);
        layout->addWidget(grayButton);
        layout->addWidget(blurButton);
        layout->addWidget(saveButton);
        layout->addWidget(imageLabel);

        connect(loadButton, &QPushButton::clicked, this, &ImageProcessor::loadImage);
        connect(grayButton, &QPushButton::clicked, this, &ImageProcessor::applyGrayscale);
        connect(blurButton, &QPushButton::clicked, this, &ImageProcessor::applyBlur);
        connect(saveButton, &QPushButton::clicked, this, &ImageProcessor::saveImage);

        setCentralWidget(centralWidget);
        setWindowTitle("Image Processor");
        resize(800, 600);
    }

    void displayImage() {
        if (!image.empty()) {
            cv::cvtColor(image, image, cv::COLOR_BGR2RGB); // Convert to RGB for display
            QImage qImage(image.data, image.cols, image.rows, image.step[0], QImage::Format_RGB888);
            imageLabel->setPixmap(QPixmap::fromImage(qImage));
        }
    }
};

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);
    ImageProcessor window;
    window.show();
    return app.exec();
}

#include "main.moc"
