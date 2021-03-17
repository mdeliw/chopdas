# Py

## Commands

```bash
# List installed modules
python3 -m pip list --format columns
python3 -m pip list --outdated

# Uninstall specific module
python3 -m pip uninstall <module-name> --yes
python3 -m pip uninstall <module-name> -y

# Uninstall multiple modules
pip3 list | grep <some wildcard text> | xargs pip3 uninstall -y

# Upgrade module
pip3 install <module-name> --upgrade
pip3 install <module-name> -u
pip3 install $(pip3 list --outdated | awk '{ print $1 }') --upgrade
pip3 install $(pip3 list --outdated --format=columns | tail -n +3 | cut -d" " -f1) --upgrade
pip3 install --upgrade numpy==1.19.1

# Sample script
#!/bin/bash
pip3 install $(pip3 list --outdated | awk '{ print $1 }') --upgrade
sudo chmod +x pip-upgrade
sudo cp pip-upgrade /usr/bin/

# One more way
for i in  $(pip3 list --outdated --format=columns | tail -n +3 | cut -d" " -f1); do pip3 install $i --upgrade; done
```

