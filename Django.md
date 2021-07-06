# Django 📟

⌚ Last Updated on: 07/06/2021

***

#### Starting a `virtualenv`

* Good practice to have a **virtual environment** (an isolated python environment where your library installations are separated from your main system's installations ).
  * Installation of `virtualenv`: `sudo apt-get install python3-venv`
  * Once you make a working directory with your app's code(and you are inside of it)- you can create your `virtualenv`:  `python3 -m venv [NAME OF ENV]`
  * To start the `virtualenv`: `source env/bin/activate`
  * Now you can do all the `pip3`s you want and they will be isolated to this `virtualenv` while its active.
  * Once you are done working in this `virtualenv`: `deactivate`

#### Django Installation (Linux)

* To install the library:`pip3 install Django`







***

### 🥽🥼 Resources Used:

* https://www.djangoproject.com/start/ (*Django Tutorials* by `Django Software Foundation`)
* https://www.youtube.com/watch?v=UmljXZIypDc (*Python Django Tutorial* by`Corey Schafer`)