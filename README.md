# Code Character Player Documentation 2020

## Setup Instructions

1. Clone this repo

2. Set up the environment :
    * Create virtual environment files - `virtualenv -p python3 venv`
    * Activate virtual environment - `source venv/bin/activate`

2. Run `sudo pip3 install -r requirements.txt`

## Build Instructions

Run `make html` in the project root to output HTML. The docs will be in `_build/html`.

Move those files to `/var/www/html` directory and visit `localhost` in browser to view the docs.