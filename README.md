# Chatbot
import nltk
import random
import requests
import re
from nltk.chat.util import Chat, reflections


patterns = [
    (r'hi|hello|hey', ['Hello!', 'Hi there!', 'Hey!']),
    (r'how are you?', ['I am doing well, thank you!', 'I am fine, thanks!']),
    (r'what is your name?', ['I am a chatbot!', 'My name is Chatbot.']),
    (r'(.*) your name?', ['My name is Chatbot.', 'You can call me Chatbot.']),
    (r'bye|goodbye', ['Goodbye!', 'See you later!', 'Bye!']),
    (r'my name is (.*)', ['Nice to meet you, {}!', 'Hello, {}!']),
    (r'I am (.*)', ['Hello, {}. How can I assist you today?', 'Hi, {}. What can I do for you?']),
    (r'(.) age(.)', ['Age is just a number! Let\'s focus on something else.']),
    (r'(.) (hungry|eat|food)(.)', ['What would you like to eat?', 'I can suggest some food options if you tell me your preferences.']),
    (r'I like (.*)', ['That\'s great!', 'Awesome!', 'Nice choice!']),
    (r'I need (.*)', ['Sure, I can help you with that. Please provide more details.']),
    (r'(.) help (.)', ['I can assist you with various things. What do you need help with?']),
    (r'(.) (movie|film)(.)', ['Do you have any specific genre in mind?', 'I can recommend some movies if you tell me your preferences.']),
    (r'(.) (weather|forecast)(.)', ['Where are you located? I can check the weather for you.']),
]


chatbot = Chat(patterns, reflections)


def get_weather(location):
    api_key = '809529cd9c113277e14d9fa37d5a2c12'  
    base_url = f'http://api.openweathermap.org/data/2.5/weather?q={location}&appid={api_key}'
    response = requests.get(base_url)
    if response.status_code == 200:
        data = response.json()
        if 'main' in data and 'temp' in data['main']:
            temperature = data['main']['temp']
            temperature_celsius = round(temperature - 273.15, 2)
            return f"The current temperature in {location} is {temperature_celsius}°C."
    return "Sorry, I couldn't retrieve the weather information for that location."


def chat_with_bot():
    print("Welcome! Start chatting with the chatbot (type 'quit' to exit)")
    while True:
        user_input = input("You: ")
        if user_input.lower() == 'quit':
            print("Chatbot: Goodbye!")
            break
        elif 'weather' in user_input.lower():
            location = input("Chatbot: Please provide your location: ")
            response = get_weather(location)
            print("Chatbot:", response)
        else:
            response = chatbot.respond(user_input)
            print("Chatbot:", response)


if __name__ == "__main__":
    nltk.download('punkt')  
    chat_with_bot()
