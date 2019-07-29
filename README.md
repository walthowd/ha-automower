# ha-automower

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/custom-components/hacs)

Automower Custom Component for Home Assistant. This is based off the work of [yeah/jan's automower custom component](
https://github.com/yeah/home-assistant).

This component is packaged for a current modern Home Assistant environment (0.93+) that will be easy to install for new users. 

# Installation Instructions
The easiest method is to use [HACS (Home Assistant Community Store)](https://custom-components.github.io/hacs/). Simply 
navigate to Settings and add this repository's URL as a Custom Repository.

Alternatively, clone or download the zip of this repository and place the `custom_components/automower` directory into your `<ha-config>/custom_components` directory. 
  
## Configuration

Add the following to your *configuration.yaml* file:

    automower:
      username: !secret husqvarna_username
      password: !secret husqvarna_password

And add these secrets to your *secrets.yaml*

    husqvarna_username: your@mail.com
    husqvarna_password: yourpassword
    
# Usage
This component will create a vacuum entity and a device_tracker entity for your automowers. The vacuum entity can be used to control the automower and view it's current status. Calling the vacuum.return_to_base service will park the automower until further notice, vacuum.stop will stop the mower in it's current location, and calling vacuum.start_pause service will resume your current schedule. 

The device_tracker.entity can be used to return the current GPS coordinates of your mower. Home Assistant by default will not show device_trackers if they are in your home zone but you can [follow the directions to add a Google Maps card](https://www.home-assistant.io/cookbook/google_maps_card/) and then adding the following to your camera config

    camera:
    - platform: generic
      name: Automower Location
      still_image_url: https://maps.googleapis.com/maps/api/staticmap?center={{ states.device_tracker.automower.attributes.latitude }},{{ states.device_tracker.automower.attributes.longitude }}&zoom=19&size=500x500&maptype=satellite&markers=color:blue%7Clabel:P%7C{{ states.device_tracker.automower.attributes.latitude }},{{ states.device_tracker.automower.attributes.longitude }}&key=YOURAPIKEY
      limit_refetch_to_url_change: true
