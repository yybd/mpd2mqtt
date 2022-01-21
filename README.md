# mpd2mqtt

You can change the payers state by, sending something to the topic music/mpd/set.

    {"player": "play"}
    {"player": "pause"}
    {"player": "toggle"}
    {"player": "next"}
    {"player": "prev"}
    {"player": "stop"}
    {"player": "update"}

It's possible to change options by:

    {"options": { "random": "on"} }
    {"options": { "replaygain": "track"} }
    {"options": { "consume": "off"} }
    {"options": { "repeat": "on"} }
    {"options": { "single": "off"} }
    {"options": { "volume": "+3"} }

You can change your queue:

    {"queue": { { "clear": "true", "add": "MixedMusic/preferredSongs", "play": "true" } } }
    {"queue": { { "clear": "false", "del": "3" } } }
    {"queue": { { "clear": "false", "insert": "MixedMusic/preferredSongs/I want to hear next.mp3", "play": "true" } } }
    
 The current options of MPD will be send in a message like:

    {
      "options": {
        "volume": "98%",
        "repeat": "on",
        "random": "off",
        "single": "off",
        "consume": "off"
      }

get music/mpd/get:
    
    {
      "player": {
        "current": {
          "name": "",
          "artist": "Billy Talent",
          "album": "Billy Talent III",
          "albumartist": "",
          "comment": "",
          "composer": "",
          "date": "2009",
          "originaldate": "%originaldate%",
          "disc": "",
          "genre": "",
          "performer": "",
          "title": "Saint Veronika",
          "track": "",
          "time": "4:10",
          "file": "Alben/Billy Talent - Billy Talent III/03 - Billy Talent - Saint Veronika.mp3",
          "position": "16",
          "id": "67",
          "prio": "0",
          "mtime": "Mon Dec 27 16:42:15 2010",
          "mdate": "12/27/10"
        },
        "state": "paused"
      }
      


To debug or check it you can use the commandline mqtt client: mosquitto_pub -h localhost -t "music/mpd/set" -m '{"player":"toggle"}'



