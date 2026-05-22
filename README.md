# hb-docs
Documentation for honey badger robots

# Building
## Prepare build environment
```
sudo apt install python3-venv
python3 -m venv venv
venv/bin/activate
pip install -r requirements.txt
```

## Building
```
jupyter-book build .
```

# Pushing & Merging
Before every merge, do a typos check with:
```
typos
```


