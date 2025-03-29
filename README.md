3-Создадим приложение на Python с использованием PyQt5 и matplotlib для демонстрации интерференции двух гармонических волн. Программа позволит пользователю вводить параметры для первых и вторых волн (амплитуда, частота и фаза) и построит графики для каждой волны, а также их результирующую интерференцию.
import sys
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QLineEdit, QPushButton

class WaveInterferencePlotter(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle('Интерференция двух гармонических волн')
        self.setGeometry(100, 100, 800, 600)

        layout = QVBoxLayout()

        # Поля ввода для первой волны
        self.amplitude1_label = QLabel('Амплитуда 1 (в м):')
        self.amplitude1_input = QLineEdit()
        layout.addWidget(self.amplitude1_label)
        layout.addWidget(self.amplitude1_input)

        self.frequency1_label = QLabel('Частота 1 (в Гц):')
        self.frequency1_input = QLineEdit()
        layout.addWidget(self.frequency1_label)
        layout.addWidget(self.frequency1_input)

        self.phase1_label = QLabel('Фаза 1 (в градусах):')
        self.phase1_input = QLineEdit()
        layout.addWidget(self.phase1_label)
        layout.addWidget(self.phase1_input)

        # Поля ввода для второй волны
        self.amplitude2_label = QLabel('Амплитуда 2 (в м):')
        self.amplitude2_input = QLineEdit()
        layout.addWidget(self.amplitude2_label)
        layout.addWidget(self.amplitude2_input)

        self.frequency2_label = QLabel('Частота 2 (в Гц):')
        self.frequency2_input = QLineEdit()
        layout.addWidget(self.frequency2_label)
        layout.addWidget(self.frequency2_input)

        self.phase2_label = QLabel('Фаза 2 (в градусах):')
        self.phase2_input = QLineEdit()
        layout.addWidget(self.phase2_label)
        layout.addWidget(self.phase2_input)

        # Кнопка построить
        self.plot_button = QPushButton('Построить интерференцию')
        self.plot_button.clicked.connect(self.plot_interference)
        layout.addWidget(self.plot_button)

        # Виджет для отображения графика
        self.canvas = FigureCanvas(plt.Figure())
        layout.addWidget(self.canvas)

        self.setLayout(layout)

    def plot_interference(self):
        # Считывание данных с полей ввода
        try:
            A1 = float(self.amplitude1_input.text())
            f1 = float(self.frequency1_input.text())
            phi1 = float(self.phase1_input.text())
            A2 = float(self.amplitude2_input.text())
            f2 = float(self.frequency2_input.text())
            phi2 = float(self.phase2_input.text())
        except ValueError:
            return  # Если введены некорректные данные, выходим из функции
        
        # Преобразование углов из градусов в радианы
        phi1_rad = np.radians(phi1)
        phi2_rad = np.radians(phi2)

        # Создание временной оси
        t = np.linspace(0, 5, 1000)  # время от 0 до 5 секунд

        # Вычисление волн
        wave1 = A1 * np.sin(2 * np.pi * f1 * t + phi1_rad)
        wave2 = A2 * np.sin(2 * np.pi * f2 * t + phi2_rad)
        resultant_wave = wave1 + wave2

        # Построение графика
        self.canvas.figure.clear()
        ax = self.canvas.figure.add_subplot(111)
        ax.plot(t, wave1, label='Волна 1', color='blue')
        ax.plot(t, wave2, label='Волна 2', color='orange')
        ax.plot(t, resultant_wave, label='Результирующая', color='green')
        ax.set_xlabel('X -- время (с)')
        ax.set_ylabel('Y -- смещение (м)')
        ax.set_title('Интерференция двух гармонических волн')
        ax.grid()
        ax.legend()

        self.canvas.draw()

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = WaveInterferencePlotter()
    window.show()
    sys.exit(app.exec_())

-- Описание работы программы
1. Программа создает графический интерфейс с полями ввода для параметров двух волн: амплитуда, частота и фаза (в градусах).
2. После нажатия на кнопку "Построить интерференцию", программа вычисляет значения для каждой волны по формуле y(t) = A * sin(2πft + φ), а также результирующую интерференцию.
3. График отображается на одном графике с подписями осей "X -- время (с)" и "Y -- смещение (м)", а также с легендой для каждой из волн.
