# selseus
Selseus is a system that is designed to automate the process of attendance marking in a post Covid era. Go to the [website](https://selseus.com) to get the app.

## What's the deal?
During the pandemic, the government made it mandatory for all institutions to allow entry only after ascertaining that the body temperature fell within the permissible range.
The same applied to educational institutions too. Our college also implemented the same on campus. However, this actually led to a long queue of students waiting there each morning just to check their temperature and write down the details in a record book. This posed many problems like the obvious health risks to the personnel involved, and to the students themselves as this queue might be a medium for the disease to spread. This was the problem we faced.

## The solution
This is a task in which we can eliminate most human staff and personnel and instead replace them with computers. Computers can also speed up this process and maintain a large historical record of temperatures of any visitors and students in the campus as a whole.
Selseus aims to achieve just this...

## selseus Mark I
We're not so creative with names, as you can see. But this is our first prototype system. But here's what it looks like as a whole. You can also watch a live demo in the launch ceremony on [Instagram](https://www.instagram.com/tv/CW6HPy3gL1k/?igshid=YmMyMTA2M2Y=). Although the presentation is in Malayalam, you could watch the thing work if you don't understand the language.

### Overview
This system consists of two user-facing endpoints: a device codenamed terminal and a mobile app. We’ll be referring to this device by it’s codename, terminal, for the rest of this README. The terminal in it’s prototype form is powered by a Raspberry Pi at its core connected to a contactless IR temperature sensor, a buzzer and a camera. There is also a display to show the GUI.
On a normal day, any student trying to enter the campus will be required to show their hand at the terminal. It will then measure their body temperature and generate a QR code for them. All this happens within a second after showing their hand. Then, the terminal will present them with two options: scan its code, or show their code.  Then, they can do either of these by using the app. Since they’ve already registered an account in the app, their details will be fetched and their attendance for that day will be marked along with their temperature.
The institution will have a separate administrator application through which they’ll be able to access the attendance records of all students. The institution will also be able to maintain a chronological record of all visitors using Selseus.

### Hardware
As you saw, the hardware means the terminal. It's essentially the following things packed together neatly:

* Raspberry Pi with a camera
* MLX90614 IR temperature sensor
* Buzzer & Push button
* A display with HDMI input

### Software
Like we already told you, there are 4 software components in the whole project, out of which 3 are currently operational. 

* [The app](https://github.com/selseus/app)
    
    It's written in React Native. Just does what it's meant to, logs in people, scans QR codes at terminals, and helps record & show their private attendance data & history. Nothing fancy. Uses an SQL DB to store this stuff. The user auth is a combo of Firebase auth and our server.
* [Terminal](https://github.com/selseus/terminal)

    It's a React app basically. There's a Python backend to communicate with the Pi. Sensor data and other hardware comms happen through there. There are also some shell scripts here and there to control the actual computer. The React app communicates with the server, records temperatures, etc.
* [Server](https://github.com/selseus/server)

    A plain Express.js server, running on an Ubuntu server via Nginx. Pretty old school, yeah, no Docker, no Kubernetes. It's based on Socket.io to enable that real time temperature based attendance marking. Uses MongoDB to store user data. Firebase auth admin API is used to make the auth work.
* Admin Panel (not operational)

    A Tauri-React app, with just enough functionality and a clean UI to simply view the records on the server. Intended for the admins & origanizations.

All of the transactions across the board are AES-256 encrypted during transit. Heck, even the QR codes are encrypted.

## Production
Hmm... Well, that's a bit iffy. Currently, the project got selected for the [Young Innovators Programme (2021)](https://yip.kerala.gov.in/). If things don't go south, a production version doesn't seem unlikely. We don't intend to make the software propreitary however. It will remain open-source.

In the production version, we intend to completely eliminate fragile components such as a separate TV and sensor and are planning to bake in a display panel and sensor into a custom PCB based on the Raspberry Pi 4. We will also eliminate the huge mounting frame being used currently and 3D print all components including the display into a compact unit.
