# ece382
ECE382 Course Web

### Automatic build
- The course website now builds and publishes automatically via GitHub Actions and GitHub Pages.  The rest of the README below pertains to building locally.

### How to install required Python packages? 
- Open a terminal.
- Navigate to the root folder and find requirements.txt
- run `pip install -r requirements.txt`
- I recommend using a virtual environment.

### How to build and publish?
- Run the following under the `docs` folder.
- Build: `jupyter-book build --all .`  
- Publish: `ghp-import -n -p -f _build/html` 

### Recommended extensions for vscode
- Python, Python Extension Pack
- Jupyter, MyST-Markdown


### Settings inside settings.json
```
{
    "python.envFile": "${workspaceFolder}/.venv",
    "python.terminal.activateEnvInCurrentTerminal": true,
    
    "cSpell.words": [
        "Gradescope",
        "microcontroller"
    ]
}
```
