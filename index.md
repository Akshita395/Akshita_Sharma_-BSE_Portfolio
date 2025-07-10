# ChatGPT Magic Storybook
The ChatGPT Magic Storybook is a complex engineering project that is designed to allow users to generate a AI created fictional story based on a user input. The project uses the capabilities of the Raspberry Pi Software System to create a display that can be coded to take user input and be sent to ChatGPT to process using an API key. However, despite the project sounding easy, I faced many technincal difficulties throughout the project, including trouble oning the Raspberry Pi, my libraries not working, and the struggle to add a keyboard system. Still, despite that, I perservered through, working around my problems by using my resources and being patient.

| **Akshita S** | **Mountain House High School** | **Computer Science | CyberSecurity** | **Incoming Senior** |

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For my final milestone, I managed to get the keyboard to function, allowing me to complete my project. At first, I tried to create a keyboard using pygame however, when that didn't work, I decided to switch to the Vkeyboard library, which was more of a success. Eventually, once I got the keyboard button to function and added to enter button, I finished my ChatGPT Magic Storybook project. Despite the struggles I faced to complete my projects, the 3 weeks I spent in BlueStamp greatly helped me grow as a engineer and coder. I learned a variety of troubleshooting techniques, terminal commands, as well as basics of the Raspberry Pi system. Additionally, I learned about the importance of perseverance and patience when dealing with challenges that are a struggle to overcome. As I continue towards developing my career, I can't wait to continue applying and growing what I learned.


# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/FI-mawsJNe4?si=_PC12QgC-3iuHHPE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my second milestone, I managed to get the necessary python libraries working for my software code. Due to my systems's unability to find the libraries, I had to manually install each and every single library myself. IT WAS TIRING. At first, I tried Pip install however when that didn't work, I decided to switch to a better package manager: Anaconda. _However, _ that also didn't work due to Anaconda NOT supporting Raspberry Pi, making use switch to Anaconda's little brother, Miniconda. Once I installed Miniconda, I reinstalled all the libraries, but then we faced _another_ issue when installing the Python Speech Recognition library. To put to perspective, the Speech Recogniction library only supports python versions 10 and 11, but I had python version 13, making the systems incompatible. Therefore, we had to switch the user input from vocal input to keyboard input, changing a key part of my project. This modification gave way for the final step in the completion of my project.

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/D3ggCFwkSOE?si=lFn47m6LgmDwdIsv" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my first milestone, I connected my computer to the raspberry pi. In the span of four days, I faced many challenging rigours of pairing my Windows desktop to the Raspberry Pi operating system. On Monday, I was unable to install the Pi operating system from the Imager due to port incompatibility, resulting in me having to wait a day for an USB cable. Later, on Wednesday, when I install the pi, I tried to SSH my computer to the raspberry pi. However, that did not work. After spending 2 days troubleshooting the problem, we solved it once I connected the raspberry pi to the display. Once connected to the display, we found out that the raspberry pi was not connecting to my home internet, resulting in me having to manually connect it. Once the WiFi was connected, I was able to establish an SSH connection with my computer, solving my week worth long struggle. Due to this accomplishment, I was allowed to start the software implementation of my project. 

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
```python
# SPDX-FileCopyrightText: 2023 Melissa LeBlanc-Williams for Adafruit Industries
#
# SPDX-License-Identifier: MIT

import threading
import sys
import os
import re
import time
import argparse
import math
import configparser
from enum import Enum
from collections import deque

import keyboardlayout as kl
import keyboardlayout.pygame as klp

#import board
#import digitalio
#import neopixel
from openai import OpenAI
import pygame
from pygame_vkeyboard import *
from rpi_backlight import Backlight
from adafruit_led_animation.animation.pulse import Pulse


# Base Path is the folder the script resides in
BASE_PATH = os.path.dirname(sys.argv[0])
if BASE_PATH != "":
    BASE_PATH += "/"

# General Settings
STORY_WORD_LENGTH = 800
#REED_SWITCH_PIN = board.D17

API_KEYS_FILE = "~/keys.txt"
PROMPT_FILE = "/boot/bookprompt.txt"

# Quit Settings (Close book QUIT_CLOSES within QUIT_TIME_PERIOD to quit)
QUIT_CLOSES = 3
QUIT_TIME_PERIOD = 5  # Time period in Seconds
QUIT_DEBOUNCE_DELAY = 0.25  # Time to wait before counting next closeing

# Neopixel Settings


# Image Settings
WELCOME_IMAGE = "welcome.png"
BACKGROUND_IMAGE = "paper_background.png"
LOADING_IMAGE = "loading.png"
BUTTON_BACK_IMAGE = "button_back.png"
BUTTON_NEXT_IMAGE = "button_next.png"
BUTTON_NEW_IMAGE = "button_new.png"
BUTTON_ENTER_IMAGE= "download.png"

# Asset Paths
IMAGES_PATH = BASE_PATH + "images/"
FONTS_PATH = BASE_PATH + "fonts/"

# Font Path & Size
TITLE_FONT = (FONTS_PATH + "Desdemona Black Regular.otf", 48)
TITLE_COLOR = (0, 0, 0)
TEXT_FONT = (FONTS_PATH + "times new roman.ttf", 24)
TEXT_COLOR = (0, 0, 0)

# Delays Settings
# Used to control the speed of the text
WORD_DELAY = 0.1
TITLE_FADE_TIME = 0.05
TITLE_FADE_STEPS = 25
TEXT_FADE_TIME = 0.25
TEXT_FADE_STEPS = 51
ALSA_ERROR_DELAY = 0.5  # Delay to wait after an ALSA errors

# Whitespace Settings (in Pixels)
PAGE_TOP_MARGIN = 20
PAGE_SIDE_MARGIN = 20
PAGE_BOTTOM_MARGIN = 0
PAGE_NAV_HEIGHT = 100
EXTRA_LINE_SPACING = 0
PARAGRAPH_SPACING = 30

# ChatGPT Parameters
SYSTEM_ROLE = "You are a master AI Storyteller that can tell a story of any length."
CHATGPT_MODEL = "gpt-3.5-turbo"  # You can also use "gpt-4", which is slower, but more accurate
WHISPER_MODEL = "whisper-1"

# Speech Recognition Parameters
ENERGY_THRESHOLD = 300  # Energy level for mic to detect
RECORD_TIMEOUT = 30  # Maximum time in seconds to wait for speech

# Do some checks and Import API keys from API_KEYS_FILE
config = configparser.ConfigParser()

#if os.geteuid() != 0:
#    print("Please run this script as root.")
#    sys.exit(1)
#username = os.environ["SUDO_USER"]
user_homedir = os.path.expanduser("/home/akshita")
API_KEYS_FILE = API_KEYS_FILE.replace("~", user_homedir)

print(os.path.expanduser(API_KEYS_FILE))
config.read(os.path.expanduser(API_KEYS_FILE))
if not config.has_section("openai"):
    print("Please make sure API_KEYS_FILE points to a valid file.")
    sys.exit(1)
if "OPENAI_API_KEY" not in config["openai"]:
    print(
        "Please make sure your API keys file contains an OPENAI_API_KEY under the openai section."
    )
    sys.exit(1)
if len(config["openai"]["OPENAI_API_KEY"]) < 10:
    print("Please set OPENAI_API_KEY in your API keys file with a valid key.")
    sys.exit(1)
openai = OpenAI(
    # This is the default and can be omitted
    api_key=config["openai"]["OPENAI_API_KEY"],
)

# Check that the prompt file exists and load it
if not os.path.isfile(PROMPT_FILE):
    print("Please make sure PROMPT_FILE points to a valid file.")
    sys.exit(1)


def strip_fancy_quotes(text):
    text = re.sub(r"[\u2018\u2019]", "'", text)
    text = re.sub(r"[\u201C\u201D]", '"', text)
    return text


class Position(Enum):
    TOP = 0
    CENTER = 1
    BOTTOM = 2
    LEFT = 3
    RIGHT = 4


class Button:
    def __init__(self, x, y, image, action, draw_function):
        self.x = x
        self.y = y
        self.image = image
        self.action = action
        self._width = self.image.get_width()
        self._height = self.image.get_height()
        self._visible = False
        self._draw_function = draw_function

    def is_in_bounds(self, position):
        x, y = position
        return (
            self.x <= x <= self.x + self.width and self.y <= y <= self.y + self.height
        )

    def show(self):
        self._draw_function(self.image, self.x, self.y)
        self._visible = True

    @property
    def width(self):
        return self._width

    @property
    def height(self):
        return self._height

    @property
    def visible(self):
        return self._visible


class Textarea:
    def __init__(self, x, y, width, height):
        self.x = x
        self.y = y
        self.width = width
        self.height = height

    @property
    def size(self):
        return {"width": self.width, "height": self.height}


class Book:
    def __init__(self, rotation=180):
        self.paragraph_number = 0
        self.page = 0
        self.pages = []
        self.stories = []
        self.story = 0
        self.rotation = rotation
        self.images = {}
        self.fonts = {}
        self.buttons = {}
        self.width = 0
        self.height = 0
        self.textarea = None
        self.screen = None
        self.saved_screen = None
        self._sleeping = False
        self.sleep_check_delay = 0.1
        self._sleep_check_thread = None
        self._sleep_request = False
        self._running = True
        self._busy = False
        self._loading = False
        self.text= ""
        self.keyboardOn= False
        self.prompt=""
        # Use a Double Ended Queue to handle the heavy lifting
        self._closing_times = deque(maxlen=QUIT_CLOSES)
        # Use a cursor to keep track of where we are in the text area
        self.cursor = {"x": 0, "y": 0}
      

        self._prompt = ""
        
    def start(self):
        # Output to the LCD instead of the console
        os.putenv("DISPLAY", ":0")


        # Initialize the display
        pygame.init()
        self.screen = pygame.display.set_mode((0, 0), pygame.FULLSCREEN)
        pygame.mouse.set_visible(False)
        self.screen.fill((255, 255, 255))
        self.width = self.screen.get_height()
        self.height = self.screen.get_width()

        # Preload welcome image and display it
        self._load_image("welcome", WELCOME_IMAGE)
        self.display_welcome()

        # Load the prompt file
        with open(PROMPT_FILE, "r") as f:
            self._prompt = f.read()

       

        # Preload remaining images
        self._load_image("background", BACKGROUND_IMAGE)
        self._load_image("loading", LOADING_IMAGE)

        # Preload fonts
        self._load_font("title", TITLE_FONT)
        self._load_font("text", TEXT_FONT)

        # Add buttons
        back_button_image = pygame.image.load(IMAGES_PATH + BUTTON_BACK_IMAGE)
        next_button_image = pygame.image.load(IMAGES_PATH + BUTTON_NEXT_IMAGE)
        new_button_image = pygame.image.load(IMAGES_PATH + BUTTON_NEW_IMAGE)
        enter_button_image = pygame.image.load(IMAGES_PATH + BUTTON_ENTER_IMAGE)
        button_spacing = (
            self.width
            - (
                back_button_image.get_width()
                + next_button_image.get_width()
                + new_button_image.get_width()
            )
        ) // 4
        button_ypos = (
            self.height
            - PAGE_NAV_HEIGHT
            + (PAGE_NAV_HEIGHT - next_button_image.get_height()) // 2
        )

        self._load_button(
            "back",
            button_spacing,
            button_ypos,
            back_button_image,
            self.previous_page,
            self._display_surface,
        )

        self._load_button(
            "enter",
            200,
            200,
            enter_button_image,
            self._make_story_prompt,
            self._display_surface,
        )

        self._load_button(
            "new",
            button_spacing * 2 + back_button_image.get_width(),
            button_ypos,
            new_button_image,
            self.new_story,
            self._display_surface,
        )

        self._load_button(
            "next",
            button_spacing * 3
            + back_button_image.get_width()
            + new_button_image.get_width(),
            button_ypos,
            next_button_image,
            self.next_page,
            self._display_surface,
        )

        # Add Text Area
        self.textarea = Textarea(
            PAGE_SIDE_MARGIN,
            PAGE_TOP_MARGIN,
            self.width - PAGE_SIDE_MARGIN * 2,
            self.height - PAGE_NAV_HEIGHT - PAGE_TOP_MARGIN - PAGE_BOTTOM_MARGIN,
        )

        # Start the sleep check thread after everything is initialized
        self._sleep_check_thread = threading.Thread(target=self._handle_sleep)
        self._sleep_check_thread.start()


    def deinit(self):
        self._running = False
    #    self._sleep_check_thread.join()
    #    self._load_thread.join()
        #self.backlight.power = True

    def _handle_sleep(self):
        #reed_switch = digitalio.DigitalInOut(REED_SWITCH_PIN)
        #reed_switch.direction = digitalio.Direction.INPUT
        #reed_switch.pull = digitalio.Pull.UP

        while self._running:
            if self._sleeping:  # Book Open
             self._wake()
            elif not self._sleeping:
             self._sleep()
            time.sleep(self.sleep_check_delay)

        while self._running:
            if self._loading:
                time.sleep(0.1)

        # Turn off the Neopixels
       
        # Handle loading color by setting the loading flag
        
        # Handle other status colors by setting the neopixels
        
    def handle_events(self):
        if not self._sleeping:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    raise SystemExit
                if event.type == pygame.MOUSEBUTTONDOWN:
                    self._handle_mousedown_event(event)
        #time.sleep(0.1)

    def _handle_mousedown_event(self, event):
        if event.button == 1:
            # If button pressed while visible, trigger action
            coords = self._rotate_mouse_pos(event.pos)
            for button in self.buttons.values():
                if button.visible and button.is_in_bounds(coords):
                    button.action()

    def _rotate_mouse_pos(self, point):
        # Recalculate the mouse position based on the rotation of the screen
        # So that we have the coordinates relative to the upper left corner of the screen
        angle = 360 - self.rotation
        y, x = point
        x -= self.width // 2
        y -= self.height // 2
        x, y = x * math.sin(math.radians(angle)) + y * math.cos(
            math.radians(angle)
        ), x * math.cos(math.radians(angle)) - y * math.sin(math.radians(angle))
        x += self.width // 2
        y += self.height // 2
        return (round(x), round(y))

    def _load_image(self, name, filename):
        try:
            image = pygame.image.load(IMAGES_PATH + filename)
            self.images[name] = image
        except pygame.error:
            pass

    def _load_button(self, name, x, y, image, action, display_surface):
        self.buttons[name] = Button(x, y, image, action, display_surface)

    def _load_font(self, name, details):
        self.fonts[name] = pygame.font.Font(details[0], details[1])

    def _display_surface(self, surface, x=0, y=0, target_surface=None):
        # Display a surface either positionally or with a specific x,y coordinate
        buffer = self._create_transparent_buffer((self.width, self.height))
        buffer.blit(surface, (x, y))
        if target_surface is None:
            buffer = pygame.transform.rotate(buffer, self.rotation)
            self.screen.blit(buffer, (0, 0))
        else:
            target_surface.blit(buffer, (0, 0))

    def _fade_in_surface(self, surface, x, y, fade_time, fade_steps=50):
        background = self._create_transparent_buffer((self.width, self.height))
        self._display_surface(self.images["background"], 0, 0, background)

        buffer = self._create_transparent_buffer(surface.get_size())
        fade_delay = round(
            fade_time / fade_steps * 1000
        )  # Time to delay in ms between each fade step

        def draw_alpha(alpha):
            buffer.blit(background, (-x, -y))
            surface.set_alpha(alpha)
            buffer.blit(surface, (0, 0))
            self._display_surface(buffer, x, y)
            pygame.display.update()

        for alpha in range(0, 255, round(255 / fade_steps)):
            draw_alpha(alpha)
            pygame.time.wait(fade_delay)
            if self._sleep_request:
                draw_alpha(255)  # Finish up quickly
                return

    def display_current_page(self):
        self._busy = True
        self._display_surface(self.images["background"], 0, 0)
        pygame.display.update()

        print(f"Loading page {self.page} of {len(self.pages)}")
        page_data = self.pages[self.page]

        # Display the title
        if page_data["title"]:
            self._display_title_text(page_data["title"])
            

        self._fade_in_surface(
            page_data["buffer"],
            self.textarea.x,
            self.textarea.y + page_data["text_position"],
            TEXT_FADE_TIME,
            TEXT_FADE_STEPS,
        )

        # Display the navigation buttons
        if self.page > 0 or self.story > 0:
            self.buttons["back"].show()
        if self.page != (len(self.pages)-1):
            self.buttons["next"].show()
            print("next is displayed")
        self.buttons["new"].show()
        print("new is displayed")
        pygame.display.update()
        while True:
            events=pygame.event.get()
            for event in events:
                if event.type == pygame.MOUSEBUTTONDOWN:
                    self._handle_mousedown_event(event)
                if event.type == pygame.QUIT:
                    print("Quit the pygame")
                    self._running= False
                    return
        self._busy = False

    @staticmethod
    def _create_transparent_buffer(size):
        if isinstance(size, (tuple, list)):
            (width, height) = size
        elif isinstance(size, dict):
            width = size["width"]
            height = size["height"]
        else:
            raise ValueError(f"Invalid size {size}. Should be tuple, list, or dict.")
        buffer = pygame.Surface((width, height), pygame.SRCALPHA, 32)
        buffer = buffer.convert_alpha()
        return buffer

    def _display_title_text(self, text, y=0):
        # Render the title as multiple lines if too big
        lines = self._wrap_text(text, self.fonts["title"], self.textarea.width)
        self.cursor["y"] = y
        delay_value = WORD_DELAY
        for line in lines:
            words = line.split(" ")
            self.cursor["x"] = (
                self.textarea.width // 2 - self.fonts["title"].size(line)[0] // 2
            )
            for word in words:
                text = self.fonts["title"].render(word + " ", True, TITLE_COLOR)
                if self._sleep_request:
                    delay_value = 0
                    self._display_surface(
                        text,
                        self.cursor["x"] + self.textarea.x,
                        self.cursor["y"] + self.textarea.y,
                    )
                else:
                    self._fade_in_surface(
                        text,
                        self.cursor["x"] + self.textarea.x,
                        self.cursor["y"] + self.textarea.y,
                        TITLE_FADE_TIME,
                        TITLE_FADE_STEPS,
                    )

                pygame.display.update()
                self.cursor["x"] += text.get_width()
                time.sleep(delay_value)
            self.cursor["y"] += self.fonts["title"].size(line)[1]

    def _title_text_height(self, text):
        lines = self._wrap_text(text, self.fonts["title"], self.textarea.width)
        height = 0
        for line in lines:
            height += self.fonts["title"].size(line)[1]
        return height

    @staticmethod
    def _wrap_text(text, font, width):
        lines = []
        line = ""
        for word in text.split(" "):
            if font.size(line + word)[0] < width:
                line += word + " "
            else:
                lines.append(line)
                line = word + " "
        lines.append(line)
        return lines

    def previous_page(self):
        if self.page > 0 or self.story > 0:
            self.page -= 1
            if self.page < 0:
                self.story -= 1
                self.load_story(self.stories[self.story])
                self.page = len(self.pages) - 1
            self.display_current_page()

    def next_page(self):
        self.page += 1
        if self.page >= len(self.pages):
            if self.story < len(self.stories) - 1:
                self.story += 1
                self.load_story(self.stories[self.story])
                self.page = 0
            else:
                self.generate_new_story()
        self.display_current_page()

    def new_story(self):
        self.generate_new_story()
        self.display_current_page()

    def display_loading(self):
        self._display_surface(self.images["loading"], 0, 0)
        pygame.display.update()

    def display_welcome(self):
        self._display_surface(self.images["welcome"], 0, 0)
        pygame.display.update()
        time.sleep(5)

    def display_message(self, message):
        self._busy = True
        self._display_surface(self.images["background"], 0, 0)
        height = self._title_text_height(message)
        self._display_title_text(message, self.height // 2 - height // 2)
        self._busy = False

    def load_story(self, story):
        # Parse out the title and story and render into pages
        self._busy = True
        self.pages = []
        if not story.startswith("Title: "):
            print("Unexpected story format from ChatGPT. Missing Title.")
            title = "A Story"
        else:
            title = story.split("Title: ")[1].split("\n\n")[0]
        page = self._add_page(title)
        paragraphs = story.split("\n\n")[1:]
        for paragraph in paragraphs:
            lines = self._wrap_text(paragraph, self.fonts["text"], self.textarea.width)
            for line in lines:
                self.cursor["x"] = 0
                text = self.fonts["text"].render(line, True, TEXT_COLOR)
                if (
                    self.cursor["y"] + self.fonts["text"].get_height()
                    > page["buffer"].get_height()
                ):
                    page = self._add_page()

                self._display_surface(
                    text, self.cursor["x"], self.cursor["y"], page["buffer"]
                )
                self.cursor["y"] += self.fonts["text"].size(line)[1]

            if self.cursor["y"] > 0:
                self.cursor["y"] += PARAGRAPH_SPACING
        print(f"Loaded story at index {self.story} with {len(self.pages)} pages")
        self._busy = False

    def _add_page(self, title=None):
        page = {
            "title" : title,
            "text_position": 0,
        }
        if title:
            page["text_position"] = self._title_text_height(title) + PARAGRAPH_SPACING
        page["buffer"] = self._create_transparent_buffer(
            (self.textarea.width, self.textarea.height - page["text_position"])
        )
        self.cursor["y"] = 0
        self.pages.append(page)
        return page
    
    
    def consumer(self, text):
        print('Current text : %s' % text)

    def keyboard_example(self, layout_name: kl.LayoutName):
        surface = pygame.display.set_mode([800, 480])
        consumer= self.consumer
        layout = VKeyboardLayout(VKeyboardLayout.QWERTY)
        print(layout)
        keyboard = VKeyboard(surface, consumer, layout, show_text=True)
        keyboard.draw(surface)
        self.buttons["enter"].show()
        print(keyboard)
        pygame.display.update()
        enterKey= keyboard.layout.get_key_at((487,362))
        print(enterKey.value)
        self.keyboardOn= True
        while self.keyboardOn:
            events=pygame.event.get()
            for event in events:
                if event.type == pygame.MOUSEBUTTONDOWN:
                    self._handle_mousedown_event(event)
                if event.type == pygame.QUIT:
                    print("Quit the pygame")
                    self._running= False
                    return
            keyboard.update(events)
            self.text= keyboard.get_text()
            print("This is self text:" + self.text)
            rects = keyboard.draw(surface)
            pygame.display.update(rects)
        keyboard.disable()
      

    def generate_new_story(self):
        self._busy = True
        self.display_message("Type the story you wish to read.")
        pygame.display.update()
        time.sleep(5)
        self.keyboard_example(kl.LayoutName.QWERTY)

        if self._sleep_request:
            self._busy = False
            time.sleep(0.2)
            return
        
        response = self._sendchat()
        if self._sleep_request:
            self._busy = False
            return
        print(response)

        self._busy = True
        self.stories.append(response)
        self.story = len(self.stories) - 1
        self.page = 0
        self._busy = False

        self.load_story(response)
        print("Story loaded")

        
    def _sleep(self):
        # Set a sleep request flag so that any busy threads know to finish up
        self._sleep_request = True
        while self._busy:
            time.sleep(0.1)
        self._sleep_request = False

        if (
            len(self._closing_times) == 0
            or (time.monotonic() - self._closing_times[-1]) > QUIT_DEBOUNCE_DELAY
        ):
            self._closing_times.append(time.monotonic())

        # Check if we've closed the book a certain number of times
        # within a certain number of seconds
        if (
            len(self._closing_times) == QUIT_CLOSES
            and self._closing_times[-1] - self._closing_times[0] < QUIT_TIME_PERIOD
        ):
            self._running = False
            return

        self._sleeping = True
        self.sleep_check_delay = 0
        #self.backlight.power = False

    def _wake(self):
        # Turn on the screen
        #self.backlight.power = True
        self.sleep_check_delay = 0.1
        self._sleeping = False

    def _make_story_prompt(self):
        request= self.text
        self.keyboardOn=False
        print("Make Story Prompt activated")
        self.prompt= self._prompt.format(
            STORY_WORD_LENGTH=STORY_WORD_LENGTH, STORY_REQUEST=request
        )
        print("request formatted")

    def _sendchat(self):
        response = ""
        print("Sending to chatGPT")
        print("Prompt: ", self.prompt)
        # Package up the text to send to ChatGPT
        stream = openai.chat.completions.create(
            model=CHATGPT_MODEL,
            messages=[
                {"role": "system", "content": SYSTEM_ROLE},
                {"role": "user", "content": self.prompt},
            ],
            stream=True,
        )

        for chunk in stream:
            if chunk.choices[0].delta.content is not None:
                response += chunk.choices[0].delta.content
            if self._sleep_request:
                return None

        # Send the heard text to ChatGPT and return the result
        return strip_fancy_quotes(response)

    @property
    def running(self):
        return self._running

    @property
    def sleeping(self):
        return self._sleeping


def parse_args():
    parser = argparse.ArgumentParser()
    # Book will only be rendered vertically for the sake of simplicity
    parser.add_argument(
        "--rotation",
        type=int,
        choices=[90, 270],
        dest="rotation",
        action="store",
        default=90,
        help="Rotate everything on the display by this amount",
    )
    return parser.parse_args()


def main(args):
    book = Book(args.rotation)
    try:
        book.start()
        while len(book.pages) == 0:
            if not book.sleeping:
                book.generate_new_story()
        book.handle_events()
        book.display_current_page()

        while book.running:
            book.handle_events()
    except KeyboardInterrupt:
        book.deinit()
        pygame.quit()
    finally:
        book.deinit()
        pygame.quit()


if __name__ == "__main__":
    main(parse_args())
```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| RasTech Raspberry Pi 4 Starter Kit | Used to install and connect Raspberry Pi | $103.99 | <a href="https://www.amazon.com/RasTech-Raspberry-Starter-Heatsink-Screwdriver/dp/B0C8LV6VNZ/ref=sr_1_4?crid=3506HY00MCGVM&dib=eyJ2IjoiMSJ9._zkM62vSQ8p7tNr88715LdMv_qHh72Je-tkF9PXEa3chDE53QT4aZu4AGAb4ihE61QY4ZD55nKF6Fp2Kfs8t7AbafM_JrlJFfHo9OB4eAVGqa0EB-7aoBQHPmhKHZ2MW8ny-Kd44bMVlVxPlTWVk5YHIN5P3uKVqrE5Dcal0rKkHny-O6Xyb5ux2AOU6OwVbkag_bqBX66RQNRrgBuz-0pS43mcx93IZTQA9R8NaJJypYU2HAycp-XicTFmyU60a01Nfm9iuyo6B9yA8ppN3OQQyJ-NQ9xyNPxfTLwkqtng.yAYpU6outhQcZmOZhN9Wb6yTw7A85CNUbXZguGInZNg&dib_tag=se&keywords=raspberry%2Bpi%2Bkit&qid=1718848547&s=electronics&sprefix=rasbperry%2Bpi%2Bkit%2Celectronics%2C83&sr=1-4&th=1 "> Link </a> |
| Magnetic Door Switch | What the item is used for | $9.99 | <a href="https://www.amazon.com/weideer-Magnetic-Surface-Normally-Contact/dp/B0BX2ZRZ8T/ref=sr_1_2?crid=31R9XWXYDU0UZ&dib=eyJ2IjoiMSJ9.KhkKOC7WxFoo6JJ2vLDRgHHS5My26-Ug6vT7JHDmUDpUdJ24EVPz0uUhN9c9M1ylM0h5E6WJL3RLjA3_KlvuPPV_VGXEZ7IQ9HYnXps2QlAN1F_2H-3hBPJ4txpSrvTe21uft2SdG1hbx_RFbKCMzlXla51-g54fcpFjhtyzh1Aqpxuy7JxVw6up1vIujE4muKh9FSsQ6crsIHOTY7aRqKcMzoAN0bcgrYNtwpvxpvk.5WVu68nz4HxkucEyWMLq2AVmZHIZQaDdAJJOZBmxgNw&dib_tag=se&keywords=l%2BEffect%2BSensor%2BDistributors%2BMagnetic%2Bcontact%2Bswitch%2Bbreadboard&qid=1749069523&sprefix=l%2Beffect%2Bsensor%2Bdistributors%2Bmagnetic%2Bcontact%2Bswitch%2Bbreadboard%2Caps%2C73&sr=8-2&th=1 "> Link </a> |
| Precision Screwdriver | What the item is used for | $7.99 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/ "> Link </a> |
| Raspberry Pi Camera Module Case | What the item is used for | $8.99 | <a href="https://www.amazon.com/gp/product/B07RWCGX5K/ref=ox_sc_act_title_1?smid=A2IAB2RW3LLT8D&th=1 "> Link </a> |
| Mini Microphone | What the item is used for | $7.96 | <a href="https://www.amazon.com/dp/B01MQ2AA0X?ref=fed_asin_title "> Link </a> |
| Funny & STEM Bundle Electronic Components Kits | What the item is used for | $13.49 | <a href="https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard%2Bkit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard%2Bkit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard%2Bkit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a&th=1 "> Link </a> |
| Raspberry Pi 7" Touch Screen Display | What the item is used for | $85.92 | <a href="https://www.amazon.com/Raspberry-Pi-7-Touchscreen-Display/dp/B0153R2A9I/ref=sr_1_3?crid=19RRZQD9S67DT&dib=eyJ2IjoiMSJ9.aL1OKpRoz8hfrCyNs7-hTz4InmT2UBG-dTaTRP4wumB4WNHvjBR5eZYoZItPArRXdzGquf8578gGkqz1o4i3DLem3hDX96jPtO7V7-UJTX7q7CgVLrvhqz8KveRxHAuC89VlHWrIoAhJzWtZyu117vYjfIh9TjHpLg_ujzObFizoE5_B8aJhHH75ltLghA969edKiyymIfA2yaYjj3MkikD-eg-yg4ClX8okxsrkWIFho4LNlolQlTH5CPbZnMGicx-W6dPuHeZ3DuLGb3AyfOr-SJzQiRH8fj4H_TELe3A.MumpIeGcltRJ5GQyNFDl1XqAMIKqOSngK_vEvB5i2TM&dib_tag=se&keywords=7+inch+raspberry+pi&qid=1748991450&s=electronics&sprefix=7+inch+raspberry+pi%2Celectronics%2C139&sr=1-3 "> Link </a> |
| USB Adapter | What the item is used for | $4.99 | <a href="https://www.amazon.com/Reader-Adapter-Camera-Memory-Wansurs/dp/B0B9R7H765/ref=sr_1_2?crid=3MEQVHP531DE&dd=B-O1-WnNnW5Lq2sO8IIkLtNGzeC2M1AGI-JCm90QnS4%2C&ddc_refnmnt=free&dib=eyJ2IjoiMSJ9.krxmOj1H39tJVM38PBmRJnW35YzDDqIGdU3F97jHt6omRE5ZhWDvwdAi_2XUcK0j3Uyi_FqplJa-eGpzJgryaMhuG3C-O-yzKA-9Ug54I7La5wYvdMrU7wkPmx-arz1CKfttxKjBwfANjnsxaw0v9TEVBRBr64OwcLzGsKLCuHH5Jp8odkzjIc_8dSk7pv45Y4GSJ_XvYZ5ETaaXI5_nZXohd1MTGkr51XsGEOvQdNI.pZB6gFBJP2fqUzkGJDyCG3sR0G0Pj4hwswTH9WdB_CY&dib_tag=se&keywords=sd+to+usb+adapter&qid=1750106002&refinements=p_101%3A19346686011&rnid=19346684011&sprefix=sd+to+usb%2Caps%2C113&sr=8-2 "> Link </a> |

# Other Resources
- [Resource 1](https://www.raspberrypi.com/software/)
- [Resource 2](https://learn.adafruit.com/magic-storybook-with-chatgpt?view=all)
- [Resource 3](https://github.com/baaron4/pygame_vkeyboard/blob/master/pygame_vkeyboard/vkeyboard.py)
