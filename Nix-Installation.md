
* Download the script

```bash
wget -O - https://github.com/StuffAnThings/qbit_manage/archive/master.tar.gz | tar xz --strip=1 "qbit_manage-master"
```

* Make it executable

```bash
chmod +x qbit_manage.py
```

* Get & Install Requirements

```bash
pip install -r requirements.txt
```

* Create Config

```bash
cd config
cp config.yml.sample config.yml
nano -e config.yml
```

* Updating

```bash
wget -O - https://github.com/StuffAnThings/qbit_manage/archive/master.tar.gz | tar xz --strip=1 "qbit_manage-master"
chmod +x qbit_manage.py
pip install -r requirements.txt
diff -ui config/config.yml config/config.yml.sample
```
