import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser
import os
from datetime import date

engine=pyttsx3.init('sapi5')
voices=engine.getProperty("voices")
print(voices[1].id)
engine.setProperty('voices', voices[0].id)
def speak(audio):
    engine.say(audio)
    engine.runAndWait()
def wishMe():
    hour=int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("good morning")
    elif hour>=12 and hour<18:
        speak("good afternoon")
    else:
        speak(" good evening!")

    speak("I AM jarwic, PLEASE TELL ME HOW MAY I HELP YOU")            
def takeCommand():
    r=sr.Recognizer()
    with sr.Microphone() as source:
        speak("Listening.....")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognizing.....")
        query = r.recognize_google(audio, language='en-in')    
        print(f"user  said:{query}\n",)
    except Exception as e:
        # print(e)
        speak("SAY THAT AGAIN PLEASE......")   
        return 
    return query
   
if __name__ == "__main__":
    wishMe()
    # while True:
    if 1:
        query = takeCommand().lower()

        if 'wikipedia' in query:
            speak('Searching wikipedia.....')
            query=query.replace("wikipedia","")
            results = wikipedia.summary(query, sentences=4)
            speak("According to wikipedia")
            print(results)
            speak(results)
        elif 'open youtube' in query:
            webbrowser.open("youtube.com")
        elif 'open google' in query:
            webbrowser.open("google.com")   
        elif 'open stack overflow' in query:
            webbrowser.open("stackoverflow.com")  
        elif 'play music' in query:
            music_dir="-------------------------"  #song directory path#
            songs=os.listdir(music_dir)
            print(songs)
            os.startfile(os.path.join(music_dir,songs[0]))  

        elif ' time ' in query:
            strTime=datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"SIR THE TIME IS {strTime}") 
        elif 'date' in query:
            strdate=date.today()
            day=date.today().strftime('%A')
            speak(f"sir the date is {strdate,day}")   