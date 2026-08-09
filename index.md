# AI Interactive Storybook
What if you could create your own stories by simply speaking into a microphone? For my project, I chose to create an AI Interactive Storybook that uses a Raspberry Pi 4, a USB microphone, a touchscreen, and OpenAI’s API to turn a spoken request into a personalized story. While building it, my little brother inspired me to add a speaker after mentioning that some words were difficult for him to pronounce, leading me to make the book read stories aloud while highlighting the lines. Building this project pushed me beyond my previous experience with robotics and fabrication and challenged me to integrate hardware, software, and AI into one working device.

```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
|Anushka Mehrotra | Suffern High School | Chemical or Biomedical Engineering | Incoming Junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/q69I5O55kFw?si=kdRVkOnzvoLxcxmL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Since my first milestone, I have continued working on my AI Interactive Storybook by completing the base project and improving both its functionality and physical design. I focused on making the storybook easier to use and making sure the Raspberry Pi, touchscreen, microphone, and software could work together as one system. I also made improvements to the enclosure, including creating an opening in the top right that allows the power backup to be removed for charging without having to take apart the entire enclosure.

One of the biggest challenges I faced was designing and constructing the enclosure for the Raspberry Pi and touchscreen. At first, I wasn't sure how to securely mount the Raspberry Pi without damaging it, so I had to troubleshoot and test several different ideas. Initially, I placed bubble wrap underneath the Raspberry Pi to secure it, and later added foam, cardboard, and paper to improve both the design and appearance of the enclosure. I also faced some issues with measurements, but I was able to troubleshoot and fix them later on.

Another challenge I encountered was with the NeoPixel. I realized that I couldn't use it without soldering, so I had to reconsider which enhancements I could add to the project. This was surprising because I had initially planned to include the NeoPixel as one of my modifications, but I had to adapt my plans based on the hardware limitations I encountered.

For my next steps, I plan to continue building on the base project by adding modifications that will make the storybook easier and more engaging for children to use. My goal for the next milestone is to include a speaker to help children understand the story, a magnetic door sensor that automatically opens the program for usage, and improving the story generation process, since it currently takes approximately five minutes to generate a story. 

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/LDwZ9YOMS1k?si=-OmTwEKo9Ekax1DY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my Bluestamp Engineering project, I'm creating an Interactive AI Storybook using Raspberry Pi and OpenAI. The project's goal is to allow the user to create personalized stories by speaking into a microphone. First, their prompt from their voice is converted into text using OpenAI's Whisper Speech Recognition Model. That text is then incorporated into a prompt that provides the AI with instructions about the story's format, length, and topic before being sent to ChatGPT, which generates a story based on the prompt. The completed story is then displayed on a touchscreen, making it similar to a kindle. The reason why I chose this project is because I already have experience with CAD design, fabrication, and robotics, but I wanted to challenge myself by learning new skills in electronics, embedded systems, and AI software integration. 

For my first milestone, I successfully created a working prototype capable of generating custom stories from a voice prompt. However, I ran into many challenges along the way. One of the biggest challenges I encountered involved the touchscreen ribbon cable. For example, while assembling the hardware, I was unaware that the connector latch needed to be opened before inserting the ribbon cable. This damaged both the ribbon cable and the connector. After that, I took the latches out of both connectors, which caused the one of my touchscreen to snap and the connector on my Raspberry Pi to be loose. After troubleshooting the issue, I replaced the damaged cable and properly reconnected the touchscreen. Another challenge I faced was with integrating OpenAi's API key. I initially believed the API key was free, which led to me facing issues while trying to execute the code. Due to this, I attempted to switch to Google's Gemini API, but encounted repeated 429 rate limit and compatability errors. After experimenting a little more, I switched back to OpenAI's API, added billing credits, and got my storybook working.

My next steps involve completing the physical assembly by mounting the Raspberry Pi to the touchscreen and placing the product inside the storybook. I also plan to expand on the project by adding NeoPixel LEDs and a magnetic sensor.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code

Here's the setup code for my project:
```python
sudo -E $(which python3) story.py

Here's the story.py code for my project:
# SPDX-FileCopyrightText: 2023 Melissa LeBlanc-Williams for Adafruit Industries
#
# SPDX-License-Identifier: MIT
import subprocess
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

import board
import digitalio
import neopixel
from openai import OpenAI
import pygame
from rpi_backlight import Backlight
from adafruit_led_animation.animation.pulse import Pulse

from listener import Listener

# Base Path is the folder the script resides in
BASE_PATH = os.path.dirname(sys.argv[0])
if BASE_PATH != "":
    BASE_PATH += "/"

# General Settings
STORY_WORD_LENGTH = 800
REED_SWITCH_PIN = board.D17
NEOPIXEL_PIN = board.D18
API_KEYS_FILE = "~/keys.txt"
PROMPT_FILE = "/boot/bookprompt.txt"

# Quit Settings (Close book QUIT_CLOSES within QUIT_TIME_PERIOD to quit)
QUIT_CLOSES = 3
QUIT_TIME_PERIOD = 5  # Time period in Seconds
QUIT_DEBOUNCE_DELAY = 0.25  # Time to wait before counting next closeing

# Neopixel Settings
NEOPIXEL_COUNT = 1
NEOPIXEL_BRIGHTNESS = 0.2
NEOPIXEL_ORDER = neopixel.GRBW
NEOPIXEL_LOADING_COLOR = (0, 255, 0, 0)  # Loading/Dreaming (Green)
NEOPIXEL_SLEEP_COLOR = (0, 0, 0, 0)  # Sleeping (Off)
NEOPIXEL_WAITING_COLOR = (255, 255, 0, 0)  # Waiting for Input (Yellow)
NEOPIXEL_READING_COLOR = (0, 0, 255, 0)  # Reading (Blue)
NEOPIXEL_PULSE_SPEED = 0.1

# Image Settings
WELCOME_IMAGE = "welcome.png"
BACKGROUND_IMAGE = "paper_background.png"
LOADING_IMAGE = "loading.png"
BUTTON_BACK_IMAGE = "button_back.png"
BUTTON_NEXT_IMAGE = "button_next.png"
BUTTON_NEW_IMAGE = "button_new.png"
BUTTON_READ_IMAGE = "button_read.png"

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

if os.geteuid() != 0:
    print("Please run this script as root.")
    sys.exit(1)
username = os.environ["SUDO_USER"]
user_homedir = os.path.expanduser(f"~{username}")
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
    def __init__(self, rotation=0):
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
        # Use a Double Ended Queue to handle the heavy lifting
        self._closing_times = deque(maxlen=QUIT_CLOSES)
        # Use a cursor to keep track of where we are in the text area
        self.cursor = {"x": 0, "y": 0}
        self.listener = None
        self.backlight = Backlight()
        self.pixels = neopixel.NeoPixel(
            NEOPIXEL_PIN,
            NEOPIXEL_COUNT,
            brightness=NEOPIXEL_BRIGHTNESS,
            pixel_order=NEOPIXEL_ORDER,
            auto_write=False,
        )
        self._prompt = ""
        self._load_thread = threading.Thread(target=self._handle_loading_status)
        self._load_thread.start()

    def start(self):
        # Output to the LCD instead of the console
        os.putenv("DISPLAY", ":0")

        self._set_status_color(NEOPIXEL_LOADING_COLOR)

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

        # Initialize the Listener
        self.listener = Listener(
            openai.api_key, ENERGY_THRESHOLD, RECORD_TIMEOUT
        )

        # Preload remaining images
        self._load_image("background", BACKGROUND_IMAGE)
        self._load_image("loading", LOADING_IMAGE)

        # Preload fonts
        self._load_font("title", TITLE_FONT)
        self._load_font("text", TEXT_FONT)
# Load button images

        BUTTON_SCALE = 0.75

        back_button_image = pygame.image.load(IMAGES_PATH + BUTTON_BACK_IMAGE)
        read_button_image = pygame.image.load(
            IMAGES_PATH + BUTTON_READ_IMAGE
        ).convert_alpha()
        next_button_image = pygame.image.load(IMAGES_PATH + BUTTON_NEXT_IMAGE)
        new_button_image = pygame.image.load(IMAGES_PATH + BUTTON_NEW_IMAGE)


        def scale_button(img):
            return pygame.transform.smoothscale(
                img,
                (
                    int(img.get_width() * BUTTON_SCALE),
                    int(img.get_height() * BUTTON_SCALE),
                ),
            )


        back_button_image = scale_button(back_button_image)
        read_button_image = scale_button(read_button_image)
        next_button_image = scale_button(next_button_image)
        new_button_image = scale_button(new_button_image)

        # Make speaker slightly larger
        read_button_image = pygame.transform.smoothscale(
            read_button_image,
            (
                int(read_button_image.get_width() * 1.25),
                int(read_button_image.get_height() * 1.25),
            ),
        )

        button_ypos = (
            self.height
            - PAGE_NAV_HEIGHT
            + (PAGE_NAV_HEIGHT - back_button_image.get_height()) // 2
        )

        margin = 25

        center = self.width // 2
        spacing = 95

        back_center = center - int(1.5 * spacing)
        speaker_center = center - 40
        plus_center = center + 35
        next_center = center + int(1.5 * spacing)

        back_x = int(back_center - back_button_image.get_width() / 2)
        read_x = int(speaker_center - read_button_image.get_width() / 2)
        new_x = int(plus_center - new_button_image.get_width() / 2)
        next_x = int(next_center - next_button_image.get_width() / 2)

        speaker_y = button_ypos - 8

        self._load_button(
            "back",
            back_x,
            button_ypos,
            back_button_image,
            self.previous_page,
            self._display_surface,
        )

        self._load_button(
            "read",
            read_x,
            speaker_y,
            read_button_image,
            self.read_story,
            self._display_surface,
        )

        self._load_button(
            "new",
            new_x,
            button_ypos,
            new_button_image,
            self.new_story,
            self._display_surface,
        )

        self._load_button(
            "next",
            next_x,
            button_ypos,
            next_button_image,
            self.next_page,
            self._display_surface,
        )

        self.textarea = Textarea(
            PAGE_SIDE_MARGIN,
            PAGE_TOP_MARGIN,
            self.width - PAGE_SIDE_MARGIN * 2,
            self.height - PAGE_NAV_HEIGHT - PAGE_TOP_MARGIN - PAGE_BOTTOM_MARGIN,
        )

        # Start sleep thread
        self._sleep_check_thread = threading.Thread(
            target=self._handle_sleep)
        self._sleep_check_thread.start()
        self._set_status_color(NEOPIXEL_READING_COLOR)

    def highlight_line(self, line_number):

        page = self.pages[self.page]

        if line_number >= len(page["lines"]):
            return

        line = page["lines"][line_number]

        # Create transparent overlay
        overlay = self._create_transparent_buffer((self.width, self.height))

        # Draw yellow rectangle
        pygame.draw.rect(
            overlay,
            (255, 255, 150),
            (
                self.textarea.x,
                self.textarea.y + page["text_position"] + line["y"],
                self.textarea.width,
                line["height"],
            ),
        )

        # Draw the text again
        text_surface = self.fonts["text"].render(
            line["text"],
            True,
            TEXT_COLOR,
        )

        overlay.blit(
            text_surface,
            (
                self.textarea.x,
                self.textarea.y + page["text_position"] + line["y"],
            ),
        )

        # Display using rotation function
        self._display_surface(overlay)

        pygame.display.update()

        # Rotate the overlay the same way as everything else
        overlay = pygame.transform.rotate(overlay, self.rotation)
        self.screen.blit(overlay, (0, 0))

        # Draw the text again on top
        text_surface = self.fonts["text"].render(line["text"], True, TEXT_COLOR)
        self._display_surface(
            text_surface,
            self.textarea.x,
            self.textarea.y + page["text_position"] + line["y"],
        )

        pygame.display.update()


    def deinit(self):
        self._running = False
        self._sleep_check_thread.join()
        self._load_thread.join()
        self.backlight.power = True

    def _handle_sleep(self):
        reed_switch = digitalio.DigitalInOut(REED_SWITCH_PIN)
        reed_switch.direction = digitalio.Direction.INPUT
        reed_switch.pull = digitalio.Pull.UP

        while self._running:
            if self._sleeping and reed_switch.value:  # Book Open
                self._wake()
            elif not self._sleeping and not reed_switch.value:
                self._sleep()
            time.sleep(self.sleep_check_delay)

    def _handle_loading_status(self):
        pulse = Pulse(
            self.pixels,
            speed=NEOPIXEL_PULSE_SPEED,
            color=NEOPIXEL_LOADING_COLOR,
            period=3,
        )

        while self._running:
            if self._loading:
                pulse.animate()
                time.sleep(0.1)

        # Turn off the Neopixels
        self.pixels.fill(0)
        self.pixels.show()

    def _set_status_color(self, status_color):
        if status_color not in [
            NEOPIXEL_READING_COLOR,
            NEOPIXEL_WAITING_COLOR,
            NEOPIXEL_SLEEP_COLOR,
            NEOPIXEL_LOADING_COLOR,
        ]:
            raise ValueError(f"Invalid status color {status_color}.")

        # Handle loading color by setting the loading flag
        self._loading = status_color == NEOPIXEL_LOADING_COLOR

        # Handle other status colors by setting the neopixels
        if status_color != NEOPIXEL_LOADING_COLOR:
            self.pixels.fill(status_color)
            self.pixels.show()

    def handle_events(self):
        if not self._sleeping:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    raise SystemExit
                if event.type == pygame.MOUSEBUTTONDOWN:
                    self._handle_mousedown_event(event)
        time.sleep(0.1)

    def _handle_mousedown_event(self, event):
        if event.button == 1:
            # If button pressed while visible, trigger action
            coords = self._rotate_mouse_pos(event.pos)
            for name, button in self.buttons.items():
                if button.visible and button.is_in_bounds(coords):
                    print(f"Pressed: {name}")
                    button.action()

    def _rotate_mouse_pos(self, point):
        # Recalculate the mouse position based on the rotation of the screen so that we have the coordinates relative to the upper left corner of the screen
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
        self.buttons["next"].show()
        self.buttons["new"].show()
        self.buttons["read"].show()

        pygame.display.update()
        self._busy = False
        def redraw_page(self):
            self._display_surface(self.images["background"], 0, 0)

            page = self.pages[self.page]

            if page["title"]:
                self._display_title_text(page["title"])

            self._display_surface(
                page["buffer"],
                self.textarea.x,
                self.textarea.y + page["text_position"],
            )

            if self.page > 0 or self.story > 0:
                self.buttons["back"].show()

            self.buttons["next"].show()
            self.buttons["new"].show()
            self.buttons["read"].show()

            pygame.display.update()

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
    def read_story(self):

        if not self.pages:
            return

        self.display_current_page()

        page = self.pages[self.page]

        # Read the whole page in the background
        text = " ".join(line["text"] for line in page["lines"])

        threading.Thread(
            target=lambda: subprocess.run(
                [
                    "/usr/bin/sudo",
                    "-u",
                    "anushkamehrotra",
                    "env",
                    "XDG_RUNTIME_DIR=/run/user/1000",
                    "espeak-ng",
                    "-v",
                    "en-sc",
                    "-s",
                    "105",
                    text,
                ]
            ),
            daemon=True,
        ).start()

        # Highlight lines while the page is being spoken
        for i, line in enumerate(page["lines"]):

            self.highlight_line(i)

            # estimate speaking time
            words = len(line["text"].split())
            pygame.time.wait(int(words * 480))

    def display_loading(self):
        self._display_surface(self.images["loading"], 0, 0)
        pygame.display.update()
        self._set_status_color(NEOPIXEL_LOADING_COLOR)

    def display_welcome(self):
        self._display_surface(self.images["welcome"], 0, 0)
        pygame.display.update()

    def display_message(self, message):
        self._busy = True
        self._display_surface(self.images["background"], 0, 0)
        height = self._title_text_height(message)
        self._display_title_text(message, self.height // 2 - height // 2)
        self._busy = False

    def load_story(self, story):
        # Parse out the title and story and render into pages
        self.current_story = story
        
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
                    text, self.cursor["x"], self.cursor["y"], page["buffer"],
                )
                page["lines"].append({
                    "text": line,
                    "x": self.cursor["x"],
                    "y": self.cursor["y"],
                    "height": text.get_height(),
                })
                self.cursor["y"] += self.fonts["text"].size(line)[1]

            if self.cursor["y"] > 0:
                self.cursor["y"] += PARAGRAPH_SPACING
        print(f"Loaded story at index {self.story} with {len(self.pages)} pages")
        self._set_status_color(NEOPIXEL_READING_COLOR)
        self._busy = False
        print(page["lines"])

    def _add_page(self, title=None):
        page = {
            "title": title,
            "text_position": 0,
            "lines": [],
        }
        if title:
            page["text_position"] = self._title_text_height(title) + PARAGRAPH_SPACING
        page["buffer"] = self._create_transparent_buffer(
            (self.textarea.width, self.textarea.height - page["text_position"])
        )
        self.cursor["y"] = 0
        self.pages.append(page)
        return page

    def generate_new_story(self):
        self._busy = True
        print("Textarea =", self.textarea)
        self.display_message("Speak aloud the story you wish to read.")

        if self._sleep_request:
            self._busy = False
            time.sleep(0.2)
            return

        def show_listening():
            # Pause for a beat because the listener doesn't
            # immediately start listening sometimes
            time.sleep(ALSA_ERROR_DELAY)
            self.pixels.fill(NEOPIXEL_WAITING_COLOR)
            self.pixels.show()

        self.listener.listen(ready_callback=show_listening)

        if self._sleep_request:
            self._busy = False
            return

        if not self.listener.speech_waiting():
            # No response from user, so return
            print("No response from user.")
            return

        story_request = self.listener.recognize()
        print(f"Whisper heard: {story_request}")
        story_prompt = self._make_story_prompt(story_request)
        self.display_loading()
        response = self._sendchat(story_prompt)
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

    def _sleep(self):
        # Set a sleep request flag so that any busy threads know to finish up
        self._sleep_request = True
        if self.listener.is_listening():
            self.listener.stop_listening()
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
        self._set_status_color(NEOPIXEL_SLEEP_COLOR)
        self.sleep_check_delay = 0
        self.backlight.power = False

    def _wake(self):
        # Turn on the screen
        self.backlight.power = True
        self.sleep_check_delay = 0.1
        self._set_status_color(NEOPIXEL_READING_COLOR)
        self._sleeping = False

    def _make_story_prompt(self, request):
        return self._prompt.format(
            STORY_WORD_LENGTH=STORY_WORD_LENGTH, STORY_REQUEST=request
        )

    def _sendchat(self, prompt):
        response = ""
        print("Sending to chatGPT")
        print("Prompt: ", prompt)
        # Package up the text to send to ChatGPT
        stream = openai.chat.completions.create(
            model=CHATGPT_MODEL,
            messages=[
                {"role": "system", "content": SYSTEM_ROLE},
                {"role": "user", "content": prompt},
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
        book.display_current_page()

        while book.running:
            book.handle_events()
    except KeyboardInterrupt:
        pass
    finally:
        book.deinit()
        pygame.quit()


if __name__ == "__main__":
    main(parse_args())

Here's my listener.py code:
# SPDX-FileCopyrightText: 2023 Melissa LeBlanc-Williams for Adafruit Industries
#
# SPDX-License-Identifier: MIT

import time

import speech_recognition as sr

class Listener:
    def __init__(
        self, api_key, energy_threshold=300, record_timeout=30
    ):
        self.listener_handle = None
        self.microphone = sr.Microphone()
        self.recognizer = sr.Recognizer()
        self.recognizer.energy_threshold = energy_threshold
        self.recognizer.dynamic_energy_threshold = False
        self.recognizer.pause_threshold = 1
        self.phrase_time = time.monotonic()
        with self.microphone as source:
            self.recognizer.adjust_for_ambient_noise(
                source
            )  # we only need to calibrate once, before we start listening
        self.record_timeout = record_timeout
        self._audio = None
        self.listener_handle = None
        self.api_key = api_key

    def listen(self, ready_callback=None):
        print("Start listening...")
        self._start_listening()
        if ready_callback:
            ready_callback()

        while (
            self.listener_handle and not self.speech_waiting()
        ):
            time.sleep(0.1)
        self.stop_listening()

    def _save_audio_callback(self, _, audio):
        print("Saving audio")
        self._audio = audio

    def _start_listening(self):
        if not self.listener_handle:
            self.listener_handle = self.recognizer.listen_in_background(
                self.microphone,
                self._save_audio_callback,
                phrase_time_limit=self.record_timeout,
            )

    def stop_listening(self, wait_for_stop=False):
        if self.listener_handle:
            self.listener_handle(wait_for_stop=wait_for_stop)
            self.listener_handle = None
        print("Stop listening...")

    def is_listening(self):
        return self.listener_handle is not None

    def speech_waiting(self):
        return self._audio is not None

    def recognize(self):
        if self._audio:
            # Transcribe the audio data to text using Whisper
            print("Recognizing...")
            attempts = 0
            while attempts < 3:
                try:
                    result = self.recognizer.recognize_whisper_api(
                        self._audio, api_key=self.api_key
                    )
                    self._audio = None
                    return result.strip()
                except sr.RequestError as e:
                    print(f"Error: {e}")
                    time.sleep(3)
                attempts += 1
                print("Retry attempt: ", attempts)
            print("Failed to recognize")
            return None
        return None

```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Raspberry Pi 4 | Main processor that controls the entire AI storybook system | $149.79 | <a href="https://www.amazon.com/RasTech-Raspberry-Starter-Heatsink-Screwdriver/dp/B0C8LV6VNZ/ref=sr_1_4?crid=3506HY00MCGVM&dib=eyJ2IjoiMSJ9._zkM62vSQ8p7tNr88715LdMv_qHh72Je-tkF9PXEa3chDE53QT4aZu4AGAb4ihE61QY4ZD55nKF6Fp2Kfs8t7AbafM_JrlJFfHo9OB4eAVGqa0EB-7aoBQHPmhKHZ2MW8ny-Kd44bMVlVxPlTWVk5YHIN5P3uKVqrE5Dcal0rKkHny-O6Xyb5ux2AOU6OwVbkag_bqBX66RQNRrgBuz-0pS43mcx93IZTQA9R8NaJJypYU2HAycp-XicTFmyU60a01Nfm9iuyo6B9yA8ppN3OQQyJ-NQ9xyNPxfTLwkqtng.yAYpU6outhQcZmOZhN9Wb6yTw7A85CNUbXZguGInZNg&dib_tag=se&keywords=raspberry%2Bpi%2Bkit&qid=1718848547&s=electronics&sprefix=rasbperry%2Bpi%2Bkit%2Celectronics%2C83&sr=1-4&th=1"> Link </a> |
| 7-Inch Touchscreen | Displays the storybook interface and generated stories | $38.99 | <a href="https://www.amazon.com/Hosyond-Touchscreen-Compatible-Capacitive-Driver-Free/dp/B0D3QB7X4Z/ref=sr_1_1?crid=ORF7VXSTLSE0&dib=eyJ2IjoiMSJ9.pOHOnV0W0xDkexguy-sSM18Ssx0wnu_V_lnRq_f9q-rmuaJPSXhF6hl2jHoMQIXxrm7zav5Nhra4FjLA_Qmvgah7JtXLaRLN8ky5Yapd_BK3PY9ffbX5po8l9fxBz9T5EUzqOHYomNxLX_Khg0yiUWfpjYFP4wV4o5M5a_RNFc7SksmXiYrKlTTEUu39PhkXqMlv97anFzH_2E0q-t79ieVGWU0hawNLVgl_aidXxg42btnpweKVkCJnygmqXyEx85f1SP4R9iSW3k18w7JjUeCAUdSJ4creTr2OgFnEq4c.P25tV5HW5mlBjdHQrDXZL7PGFo51qM3loPV3WmGdikQ&dib_tag=se&keywords=7%2Binch%2Braspberry%2Bpi&qid=1783981822&s=electronics&sprefix=7%2Binch%2Braspberry%2Bpi%2Celectronics%2C150&sr=1-1&th=1"> Link </a> |
| USB Microphone | Captures the user's voice for speech recognition and story generation | $7.96 | <a href="https://www.amazon.com/dp/B01MQ2AA0X?ref=fed_asin_title"> Link </a> |
| USB Mini Speaker | Plays generated stories aloud using text-to-speech | $13.99 | <a href="https://www.amazon.com/HONKYOB-Speaker-Computer-Multimedia-Notebook/dp/B075M7FHM1/ref=sr_1_5?dib=eyJ2IjoiMSJ9.6xySeP2KQ2waeO2PCFKgXydSBPn8zMGbV4f0PpDrivi1dSXmFsMVWaDtYKiBvAzMnpLJfOjboj8se6MFz1OV03KjbftCDQ-kTXDaa2axi2rkLhPa3au-lAEWRBmxKWs3p4GzpXMk6Fzp4ZXpevEsHVXieKvakfR7VCIyboe3c9MlDcxwdTCoy1EsykYr7CajrIe2NWaoY51ZzYqThDSe9-FTS8TuFBc0KwUG82fqJIs.FekNzrO6LkQMoaFuYFigpm0bYCCa5ARugXPSK08ndTs&dib_tag=se&keywords=speaker%2Bfor%2Braspberry%2Bpi&qid=1779543160&sr=8-5&th=1"> Link </a> |
| Magnetic Door Contact Sensor | Detects when the storybook is opened and activates the system | $9.99 | <a href="https://www.amazon.com/weideer-Magnetic-Surface-Normally-Contact/dp/B0BX2ZRZ8T/ref=sr_1_2?crid=31R9XWXYDU0UZ&dib=eyJ2IjoiMSJ9.KhkKOC7WxFoo6JJ2vLDRgHHS5My26-Ug6vT7JHDmUDpUdJ24EVPz0uUhN9c9M1ylM0h5E6WJL3RLjA3_KlvuPPV_VGXEZ7IQ9HYnXps2QlAN1F_2H-3hBPJ4txpSrvTe21uft2SdG1hbx_RFbKCMzlXla51-g54fcpFjhtyzh1Aqpxuy7JxVw6up1vIujE4muKh9FSsQ6crsIHOTY7aRqKcMzoAN0bcgrYNtwpvxpvk.5WVu68nz4HxkucEyWMLq2AVmZHIZQaDdAJJOZBmxgNw&dib_tag=se&keywords=l%2BEffect%2BSensor%2BDistributors%2BMagnetic%2Bcontact%2Bswitch%2Bbreadboard&qid=1749069523&sprefix=l%2Beffect%2Bsensor%2Bdistributors%2Bmagnetic%2Bcontact%2Bswitch%2Bbreadboard%2Caps%2C73&sr=8-2&th=1"> Link </a> |
| Fake Book Box | Physical storybook enclosure for electronics | $23.52 | <a href="https://www.amazon.com/dp/B0C4NKDWW6?lv=shuf&crid=12VYO3XQTCU1A&keywords=hollowed%20out%20book&sprefix=hollowed%20,aps,187&dib_tag=se&dib=eyJ2IjoiMSJ9.__udwxkKzpZUID8v1E5vEeuw0b7gPYz0TyNFwh5vPBCzVyYW8lnLBRLRzwpAz1Q1nON-7JrOfDpBIvXi92fCqy4JEkEeehv7uVKhMns2V9VQJL0D83FfZ6pOOoXFKYxS-9w8K6ZuaBubarzTSTM66lE1vU5rApFi8zfjGrM12NCmgUIfCAAJ1ungkIExPIsMfoP9EW-PNGeKF0CbfHSuOA2adoa9YEq4hBWZQqjJm7hcTUiOAV_RzcRAoDzl9L6MZ3U3OmHhgi9FFcoRPWd_xfjF_ebro2BdoyjF5ediedU.-oCXKxXKLSmnv7P1F3d4iNpSW_KlXwUZdvAb8G1Yjmg&qid=1750951553&sr=8-17&channelId=500&ref_=sr_1_17&plpRedirect=mhFallback"> Link </a> |
| Power Bank | Powers the storybook for portable operation | $32.99 | <a href="https://www.amazon.com/SIXTHGU-Portable-Charger-Charging-Flashlight/dp/B0C7PHKKNK/ref=sr_1_2_sspa?crid=2ZZM4AAZMMWHQ&dib=eyJ2IjoiMSJ9.W2Zx5_I3mKOn6UpwAzOw6PD0PNh1iaMRBiedequdv9weeWL0HPyPcxJBR9h6-LiFW-sHKnHSApN0sUxx0Q9xIRs80R57IlvvCsmEzXcktogo-4nP-NxrEZOy5dJTcXY8N-PBwfGt4fl_9LP8npenzDUV9TPA8KN6DMu175g6JegC_gZhAJrbqX94EfpQhLwP9vIJH45w2N-AFrfZZOy9jqk55gzVyk4Qst8uZvqn768.KBrc5_SqZ4e8zCpoFc-1C7rk02t3o2ykgDPB65W5JJU&dib_tag=se&keywords=always%2Bon%2Bpower%2Bbank&qid=1715957917&sprefix=always%2Bon%2Bpower%2Bbank%2Caps%2C107&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Digital Multimeter | Measures voltage and current during testing | $11.98 | <a href="https://www.amazon.com/gp/product/B0CXM242J1/ref=sw_img_1?smid=A34MWHFZRUFCUF&th=1"> Link </a> |
| Electronics Component Kit | Electronic parts for building and testing | $14.99 | <a href="https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard%2Bkit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard%2Bkit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard%2Bkit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a&th=1"> Link </a> |
| Screwdriver Set | Used to assemble and secure project hardware | $7.99 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
