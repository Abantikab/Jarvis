# Jarvis
import sys
import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser
import pywhatkit
import pyjokes
import pyaudio

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def take_command():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening....")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognising..")
        query = r.recognize_google(audio)
        print("User said: " + query + "\n")
    except Exception as e:
        # print(e)
        print("Say that again please....")
        return "None"
    return query


def wishme():
    hour = int(datetime.datetime.now().hour)
    if 0 <= hour < 12:
        speak('Good Morning')
    elif 12 <= hour < 18:
        speak('Good Afternoon')
    else:
        speak("Good evening")
    speak('I am Jarvis! Please tell me how can I help you?')


if __name__ == "__main__":
    wishme()
    while True:
        query = take_command().lower()

        if 'search' in query:
            speak("searching...")
            query.replace('search', '')
            results = wikipedia.summary(query, sentences=2)
            speak("According to wikipedia,"+results)
        elif 'open youtube' in query:
            webbrowser.open("youtube.com")
        elif 'open google' in query:
            webbrowser.open("google.com")
        elif 'open stackoverflow' in query:
            webbrowser.open("stackoverflow.com")
        elif 'play' in query:
            song = query.replace('play','')
            pywhatkit.playonyt(query)
        elif 'time' in query:
            strTime = datetime.datetime.now().strftime("%I:%M %p")
            print("The time is "+strTime)
            speak("The time is "+strTime)
        elif 'joke' in query:
            jk = pyjokes.get_joke()
            print(jk)
            speak(jk)
        elif 'bye' or 'stop' in query:
            speak("Thankyou for using Jarvis! Have a nice day.")
            sys.exit()
