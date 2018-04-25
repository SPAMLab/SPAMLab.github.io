---
layout: post
published: false
title: '"Garcia Garden" quarry '
subtitle: 3D modeling a vertical wall with SfM and a UAV
---

On March 28th and April 3rd we had two nice field trips to the "Garcia Garden" quarry. The goal was to acquire images with our drone to create a 3D model of the wall. The model will be used by Camila and Fernanda to determine the attitude (dip direction, dip) of fracture systems and later these will be compared with values obtained in the field. 

The place is an inactive basalt quarry, now managed by the Municipality of Campinas, and is a local hotspot for rock climbing, drone flying and jogging. Shaped like a wide "U", it has one long East-West wall and two smaller North-South walls. Since we are in the Southern Hemisphere, the Sun goes from northeast to Northwest, so the E-W wall is always under the shadows, and the West wall will be lit in the morning and dark in the afternoons (and vice-versa for the East wall). This means that we need to leave early (Campinas is about 1 hour driving from the USP campus) and work fast before the West wall gets too dark. 

We started by placing three fixed points on the ground: one in front of the wall, one a bit South of the first and one at the other side of the quarry. These points were surveyed with dGPS and we set up our Total Station over the first point to measure the orientation of recognizable features on the wal (we used climbing anchors). Back at the office, we used the dGPS coordinates and the Total Station data to calculate the real world (UTM) XYZ coordinates of the surveyed points. The coordinates will be used later to georreference the 3D model.  

| ![totalstation]({{site.baseurl}}/img/total_station.jpg) |
|:--:| 
| *Location of dGPS, Total Station and points surveyed on the wall* |

The next step is to acquire images of the wall with the drone. And that's the tricky part. The DJI drones can be programed to fly autonomous missions, but most applications are designed to fly in a grid or cross-grid pattern, which will work fine if what you want is a 2D orthophoto or a 3D model of a landscape. 

In the case of a vertical wall, you can fly mannually, but that will result in longer flight times (and more battery consumption) and it's harder to ensure a proper overlapping of images. 

After a lot of googling, we decided to plan our missions using the Lichi App, which is designed for video and photography, meaning that you can set waypoints and assign actions to each point, like taking a photo, rotating the aircraft etc. Litchi is also nice because you can plan your missions using a web-based app on the confort of your desktop's big screen and just save them for later. 

We planned a mission running parallel to the West wall with waypoints 8 meters apart (giving us an horizontal overlap of 80%) and taking 6 photos at each point: one facing the wall, one looking 15º to the right, one looking 15º to the left and another three with the same orientation but with the camera pointed 15º down. Then it was easy to duplicate the mission and change the elevation of the drone to a level 6 meters higher (vertical overlap of 80%).  

| ![gearth]({{site.baseurl}}/img/gearth_3d.png) |
|:--:| 
| *Missions planned with Litchi* |


In the end, we collected about 450 pictures of the wall and processed with Photoscan Pro (with the 'high' setting). The results look really good, and we can see a lot of detail on the wall, including small fractures and some overhanging blocks. Now we need to use the Total Station points to georreference the model to real world coordinates and go back to the quarry to collect structural data (orientation) of faults and fractures using the traditional methods (compass/tape), so we can compare them with the same data extracted from the 3D model. 

|![solid]({{site.baseurl}}/img/garcia_high_solid.jpg)|
|:--:| 
| *3D model, solid view* |  



| ![texture]({{site.baseurl}}/img/garcia_high_texture.jpg)|
|:--:| 
| *3D model, textured* |

