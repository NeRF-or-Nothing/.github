# Welcome to NeRF-or-Nothing

# What is NeRF?
In the last two years there has been an explosion of research surrounding NeRFs, or Neural Radiance Fields, as a new way to render 3D scenes from a set of 2D images. This technology is rapidly developing, and it may be the best path forwards to photorealistic rendering of viewpoints never before seen before by a camera. Many large research labs like Facebook AI and Google research are pursuing this technology in order to render realistic simulations for self-driving cars and to improve the usability of 3D scans. Our project, NeRF or Nothing, will be a web application based on this technology that allows people to input videos or collections of photos and render novel realistic views of the scene they captured. This will include the ability for users to create “flythroughs” or move virtual cameras through scene's they have captured to create videos from unseen perspectives.
  
  
  # High level overview
 In general, the project consists of the frontend website that takes the input video from user, a backend server that is implemented with TensorF and Colmap that process the video and creates the 3D fly through of the object, and a web server that is builded with flask that serves as a database to connect and pass through data between the frontend and backend. Lastly, the connections between all the branches will the a set of HTTP requests. 

In the beginning, The user uploads a video to the frontend website, that video will then be send to the Web server, more specifically the MongoDB database. Then, the Flask web server will send the input video to RabbitMQ, which is an asynchronies messenger that hosts two of the queue for processing the video. The video will first be send to the structure for motion queue and ready for the structure for motion worker to process the worker to process the video, which will take approximately 10 -15 minutes. Then, the video and the process data will be send out of the SFM queue, and goes into the NerF queue and ready for the NerF worker to process. When the 3D fly through object is created with the NeRF worker, the 3D fly through object will be send out of the RabbitMQ, and back to Web server, sorted in the MongoDB database ready for the Front-end VueJS to access, and displayed in the page.

![Full_Project](https://user-images.githubusercontent.com/75590646/185227317-ca5ffe32-1e6b-45c9-8aae-7e0515cea1a4.png)

# Front-end Structure and Framwork -- Vue.js with i18n plugin
For the website portion of this project, we chose to use the java script framework called Vue to host all the components for the website. Compare to other popular frameworks, Vue is a lot more popular, and relatively easy to get started for beginners, also, it support a wide range of javascript libraries that could be useful for the project. The i18n plugin as mentioned is to let the website have multi-language support, as that can let more users around the world have a taste of what Nerf technology is all about.

The purpose of the Frontend is to create a user interface that first can display information about the technology, more importantly, it servers as an input source for users to input the videos that he/she wants to create 3d object for. Therefore, it’s crucial for the website to have the ability to take videos from user input, and send it to the backend for processing via the web server.

# Webserver Structure and Framwork -- Flask with MongDB and RabbitMQ
There are three components that makes up the web server branch, Flask web server, MongoDB database. First, the Flask web server serves as a connection point that manages messages transmitting between the frontend and the backend, such as when the backend is processing the video in the Structure for motion queue, the flask web server will constantly get information about the time remaining for completion and feet that to the frontend to let the users know the time remaining.

Second, MongoDB is a asynchronous messenger that hoses the structure for motion queue and NerF queue for backend video processing; it’s used to manage the input video is first send to the SFM queue for processing, and when it’s done, it’s send to the NerF queue for further processing.

Lastly, the MongoDB database servers as a place to hold the input videos and output fly through videos, as such implementation is easier for the frontend and backend to access the needed resources.

![Webserver](https://user-images.githubusercontent.com/75590646/185227939-7aff7140-6831-45be-819c-e6c4ec418893.png)


# Back-end Structure and Framwork -- NerF Technology via COLMAP and Structure for motion implemented by TensorRF
In a general, NerF is a neural network that generates novel views of complex 3D scenes with 2d images and their coordinates. It works by taking input images representing a scene and interpolating between them to render one complete scene. A NeRF uses a sparse set of input views to optimize a continuous volumetric scene function. The result of this optimization is the ability to produce novel views of a complex scene.
![Worker_](https://user-images.githubusercontent.com/75590646/185227619-aac5918a-e435-4c4a-993f-277f244f2c21.png)


