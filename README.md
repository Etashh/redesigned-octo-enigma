import streamlit as st
import pyowm

def main():

    st.set_page_config(page_title="Weather", layout="wide")
    st.title("Weather")

    owm = pyowm.OWM('bd414ff0237b69d77195c9ca93f1be5b')

    location = st.text_input("Enter rough geographic location:- ")
    with st.spinner("Loading..."):

        if st.button("Display Weather"):
            if location.strip() == "":
                st.warning("Please enter a valid location.")
            else:
                try:
                    observation = owm.weather_manager().weather_at_place(location)
                    weather = observation.weather
                    temperature = weather.temperature('celsius')['temp']
                    humidity = weather.humidity
                    status = weather.status


                    st.write(f"Weather in {location}:")
                    st.write(f"Temperature: {temperature}°C")
                    st.write(f"Humidity: {humidity}%")
                    st.write(f"Status: {status}")
                except pyowm.exceptions.APIRequestError:
                    st.warning("An error occurred while fetching weather data. Please try again.")


if __name__ == "__main__":
    main()
