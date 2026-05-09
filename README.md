# ================================
# SKY VOICE ASSISTANT (AZURE AI)
# ================================

import azure.cognitiveservices.speech as speechsdk
import datetime
import webbrowser
import random
import time
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# --------------------------
# AZURE CONFIGURATION
# --------------------------

AZURE_REGION = "centralindia"  # e.g. "eastus"

speech_config = speechsdk.SpeechConfig(subscription=AZURE_SPEECH_KEY, region=AZURE_REGION)
speech_config.speech_recognition_language = "en-IN"
speech_config.speech_synthesis_voice_name = "en-US-JennyNeural"  # Female voice

# Initialize synthesizer
speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config)

def speak(text):
    """Speak text using Azure Text-to-Speech"""
    print(f"Sky: {text}")
    speech_synthesizer.speak_text_async(text)

def listen():
    """Listen using Azure Speech-to-Text"""
    recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config)
    print("Listening...")
    result = recognizer.recognize_once_async().get()
    if result.reason == speechsdk.ResultReason.RecognizedSpeech:
        print(f"You said: {result.text}")
        return result.text.lower()
    else:
        print("Didn't catch that.")
        return ""

# --------------------------
# FEATURES
# --------------------------
class InfoFetcher:
    def get_info(self, query):
        try:
            driver = webdriver.Chrome(service=Service())
            driver.get("https://www.wikipedia.org")
            wait = WebDriverWait(driver, 10)
            search = wait.until(EC.presence_of_element_located((By.ID, "searchInput")))
            search.send_keys(query)
            button = wait.until(EC.element_to_be_clickable((By.XPATH, '//*[@id="search-form"]/fieldset/button')))
            button.click()
            driver.execute_script("alert('Wikipedia page loaded. You can read it now. Close this tab when done.');")
        except Exception as e:
            print("Wikipedia Error:", e)

class YouTubePlayer:
    def play(self, query):
        try:
            options = Options()
            options.add_argument("--start-maximized")
            driver = webdriver.Chrome(service=Service(), options=options)
            driver.get(f"https://www.youtube.com/results?search_query={query}")
            wait = WebDriverWait(driver, 10)
            video = wait.until(EC.element_to_be_clickable((By.ID, "video-title")))
            video.click()
            print(f"Now playing: {query}")
        except Exception as e:
            print("YouTube Error:", e)

# --------------------------
# SKY MAIN ASSISTANT
# --------------------------
class SkyAzureAssistant:
    def __init__(self):
        self.wake_word = "hello sky"
        self.listening = False

    def get_time(self):
        return datetime.datetime.now().strftime("It's %I:%M %p")

    def get_date(self):
        return datetime.datetime.now().strftime("Today is %A, %B %d, %Y")

    def tell_joke(self):
        jokes = [
            "Why did the scarecrow win an award? Because he was outstanding in his field!",
            "Why don’t scientists trust atoms? Because they make up everything!",
            "Why don’t eggs tell jokes? They’d crack each other up!"
        ]
        return random.choice(jokes)

    def process_command(self, command):
        command = command.lower()
        if "time" in command:
            return self.get_time(), False
        elif "date" in command:
            return self.get_date(), False
        elif "search google for" in command:
            query = command.replace("search google for", "").strip()
            webbrowser.open(f"https://www.google.com/search?q={query}")
            return f"Searching Google for {query}", False
        elif "search youtube for" in command:
            query = command.replace("search youtube for", "").strip()
            webbrowser.open(f"https://www.youtube.com/results?search_query={query}")
            return f"Searching YouTube for {query}", False
        elif "joke" in command:
            return self.tell_joke(), False
        elif "open youtube" in command:
            webbrowser.open("https://www.youtube.com")
            return "Opening YouTube.", False
        elif "open google" in command:
            webbrowser.open("https://www.google.com/")
            return "Opening Google.", False
        elif "open gmail" in command:
            webbrowser.open("https://mail.google.com")
            return "Opening Gmail.", False
        elif "who are you" in command:
            return "I am Sky, your Azure-powered voice assistant.", False
        elif "wikipedia" in command or "information" in command:
            speak("What topic?")
            topic = listen()
            if topic:
                InfoFetcher().get_info(topic)
                return f"Opened Wikipedia page for {topic}", False
            return "No topic heard.", False
        elif command.startswith("play "):
            video_query = command.replace("play", "").strip()
            YouTubePlayer().play(video_query)
            return f"Playing {video_query} on YouTube", False
        elif any(word in command for word in ["exit", "bye", "quit"]):
            return "Goodbye! Have a great day!", True
        elif "help" in command:
            return """I can:
- Tell time and date
- Search Google or YouTube
- Open Gmail or YouTube
- Fetch Wikipedia info
- Play YouTube videos
- Tell jokes
Say 'hello sky' to start.""", False
        else:
            return "I didn't understand. Say 'help' to know more.", False

    def run(self):
        speak("Sky is ready. Say 'hello sky' to activate me.")
        while True:
            if not self.listening:
                print("Waiting for wake word...")
                text = listen()
                if self.wake_word in text:
                    speak("How can I help you?")
                    self.listening = True
            else:
                command = listen()
                if command:
                    response, should_exit = self.process_command(command)
                    speak(response)
                    if should_exit:
                        break
                self.listening = False


if __name__ == "__main__":
    try:
        SkyAzureAssistant().run()
    except KeyboardInterrupt:
        print("\nAssistant terminated.")
