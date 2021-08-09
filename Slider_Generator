# This script takes care of generating a window with a series of sliders that can be specified
# it automatically spaces them and adds the next slider below.

import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QSlider, QLabel
from PyQt5.QtCore import Qt


# Class that holds all the crucial parameters relative to a slider
class Slider:
    def __init__(self, name='Slider 1', min_value=0, max_value=100, step_size=1, tick_number=5, starting_value=0):
        self.name = name
        self.min_value = min_value
        self.max_value = max_value
        self.step_size = step_size
        self.tick_interval = int((max_value - min_value) / (tick_number + 1))
        self.starting_value = starting_value


# This class takes a list of Slider class instances and generates a Qwindow with all the sliders
# tightly packed. It also provides real time interaction capabilities
class SliderWindow(QMainWindow):

    # Initialize by providing a list of Slider instances to be added to the window
    def __init__(self, slider_list, debugging=False):
        super().__init__()

        label_max_length = 0
        spacing = 30
        self.label_list = list()
        self.slider_name_list = list()
        self.slider_list = list()

        self.debugging = debugging

        for index, slider in enumerate(slider_list):
            if len(slider.name) > label_max_length:
                label_max_length = len(slider.name)

            self.slider_name_list.append(slider.name)

            # For each slider object, append a new label instance to the label list
            # and adapt them to the window for proper legibility
            self.label_list.append(QLabel(self))
            self.label_list[index].resize(len(slider.name) * 10, 20)
            self.label_list[index].setText(str(slider.starting_value) + '  .' + str(slider.name))
            self.label_list[index].move(240, 38 + spacing * index)

            # For each slider object, append a new Qslider instance to the slider list and set the geometry
            # according to the parameters specified by the Slider class extracted from each of the instances
            self.slider_list.append(QSlider(Qt.Horizontal, self))
            self.slider_list[index].setGeometry(30, 40 + spacing * index, 200, 30)
            self.slider_list[index].setMinimum(slider.min_value)
            self.slider_list[index].setMaximum(slider.max_value)
            self.slider_list[index].setSingleStep(slider.step_size)
            self.slider_list[index].setTickInterval(slider.tick_interval)
            self.slider_list[index].setTickPosition(QSlider.TicksBelow)
            self.slider_list[index].setValue(slider.starting_value)
            self.slider_list[index].valueChanged.connect(self.change_value)

        # Set the geometry and title of the general window and show it
        self.setGeometry(50, 50, 350 + label_max_length * 4, 80 + spacing * len(slider_list))
        self.setWindowTitle("Parameter variables")
        self.show()

    # At any given value change, update the labels of the sliders with their current exact value
    def change_value(self):
        for index, slider_n in enumerate(self.slider_list):
            slider_label = self.label_list[index].text().split('.', 1)[1]
            slider_label = str(self.slider_list[index].value()) + '  .' + slider_label
            self.label_list[index].setText(slider_label)

        # Print an update if we are debugging
        if self.debugging:
            slider_value_list = [slider.value() for slider in slider_window.slider_list]
            slider_name_list = [name for name in slider_window.slider_name_list]
            print(list(zip(slider_value_list, slider_name_list)))


if __name__ == '__main__':
    app = QApplication(sys.argv)

    # We define the sliders we want to calibrate the software with the Slider class
    Brightness_Slider = Slider('Brightness', 0, 255, starting_value=242)
    Contrast_Slider = Slider('Contrast', 0, 255, starting_value=255)
    Saturation_Slider = Slider('Saturation', 0, 255, starting_value=0)
    Hue_Slider = Slider('Hue', 0, 20, starting_value=20)
    Gain_Slider = Slider('Gain', 0, 127, starting_value=78)
    Exposure_Slider = Slider('Exposure', -7, -1, starting_value=-7)
    White_Balance_Slider = Slider('White_Balance', 4000, 7000, starting_value=5000)

    # Now we define the main window with all the desired sliders as a list
    # We turn the debugging on to see the output
    slider_window = SliderWindow([Brightness_Slider,
                                  Contrast_Slider,
                                  Saturation_Slider,
                                  Hue_Slider,
                                  Gain_Slider,
                                  Exposure_Slider,
                                  White_Balance_Slider],
                                 debugging=True)

    # We run the application
    app.exec_()

