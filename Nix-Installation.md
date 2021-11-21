# Nix Installation
Nix Installation

* Download the script

```bash
wget -O qbit_manage.py 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/qbit_manage.py'
```

* Make it executable

```bash
chmod +x qbit_manage.py
```

* Get & Install Requirements

```bash
wget -O requirements.txt 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/requirements.txt'
pip install -r requirements.txt
```

* Get Example Config

```bash
wget -O config.yml.sample 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/config.yml.sample'
```

* Create Config

```bash
cp config.yml.sample config.yml
nano -e config.yml
```

* Updating

```bash
wget -O qbit_manage.py 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/qbit_manage.py'
chmod +x qbit_manage.py
wget -O requirements.txt 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/requirements.txt'
pip install -r requirements.txt
wget -O config.yml.sample 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/config.yml.sample'
diff -ui config.yml config.yml.sample
```
