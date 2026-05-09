# sky-voice-assistant
An AI-powered Python voice assistant using Azure Cognitive Services for speech recognition and text-to-speech. Features include Google/YouTube search, Wikipedia info fetching, YouTube playback, jokes, wake-word activation, and voice-controlled automation.
Sky Voice Assistant is a Python-based AI voice assistant powered by Microsoft Azure Cognitive Services. It uses Azure Speech-to-Text and Text-to-Speech for real-time voice interaction and supports tasks like web searching, opening websites, fetching Wikipedia information, playing YouTube videos, telling jokes, and more.

## 🚀 Features

* 🎤 Voice Recognition using Azure Speech-to-Text
* 🔊 Natural Voice Responses using Azure Text-to-Speech
* ⏰ Tell current time and date
* 🌐 Search Google and YouTube
* 📺 Play YouTube videos automatically
* 📖 Fetch Wikipedia information
* 📧 Open Gmail, Google, and YouTube
* 😂 Built-in joke generator
* 🗣️ Wake-word activation (“Hello Sky”)

## 🛠️ Technologies Used

* Python
* Azure Cognitive Services
* Selenium WebDriver
* ChromeDriver
* Webbrowser module

## 📦 Installation

1. Clone the repository
2. Install dependencies:

```bash
pip install azure-cognitiveservices-speech selenium
```

3. Download and configure ChromeDriver
4. Add your Azure Speech API Key and Region
5. Run the project:

```bash
python sky_assistant.py
```

## 🎯 How It Works

* The assistant continuously listens for the wake word:

```text
"Hello Sky"
```

* Once activated, it processes voice commands and performs tasks instantly.

## 📌 Example Commands

* “What’s the time?”
* “Search Google for Python tutorials”
* “Play lo-fi music”
* “Open Gmail”
* “Tell me a joke”
* “Wikipedia”

## 🔐 Note

Make sure to keep your Azure API keys secure and never expose them publicly in production repositories.

## 📄 License

This project is open-source and available under the MIT License.
