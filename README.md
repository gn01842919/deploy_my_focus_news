# deploy_my_focus_news

自動化佈署 [my_focus_news](https://github.com/gn01842919/my_focus_news) + [news_scraper](https://github.com/gn01842919/news_scraper) 。


## 佈署方式 (Linux)

1. 安裝 Docker、Docker Compose，以及 Git

2. 依環境與需求調整設定黨，包括:
    1. Django 設定檔 ([my_focus_news/my_focus_news/settings.py](https://github.com/gn01842919/my_focus_news/settings.py))
    2. news_scraper 設定檔 ([news_scraper/settings.py](https://github.com/gn01842919/news_scraper/blob/master/settings.py))
    3. Docker Compose 設定檔 ([my_focus_news/deployment/docker/docker-compose.yml](./docker/docker-compose.yml))
    4. [deploy.sh](./deploy.sh) 中會覆寫上述設定檔的部分設定 (讓開發環境與佈署環境不同)，需要依實際情況調整。目前會/可能被覆寫的設定有:
          1. **Django**: DEBUG, ALLOWED_HOSTS, DATABASES["default"]["PASSWORD"]
          2. **news_scraper**: DATABASE_CONFIG["db_password"], SCRAPER_CONFIG["error_log"]
          3. **docker-compose**: POSTGRES_PASSWORD，以及 web 與 scraper 的 volumes 設定

3. . 執行 `sh deploy.sh setup`
    - 建議先將 host name 與 database 寫入環境變數 SITE_HOSTNAME 與 DB_PASSWORD，此程式會將其讀出來，並覆蓋程式中的設定。例如: `export SITE_HOSTNAME="my.hostname.com"; export DB_PASSWORD="database_password" ; sh deploy.sh setup`
    - 若前述兩個環境變數為空，程式會要求使用者輸入。兩者皆可接受空白輸入，若 host name 為空白，會自動抓取環境中的 IP。若密碼為空白，則使用程式碼中寫死的資料庫密碼。
    - 以下設定無論如何都會被覆寫:
        1. **Django**: DEBUG ==> False
        2. **news_scraper**: ERROR_LOG ==> '/src/scraper_error.log'
        3. **docker-compose**: web 與 scraper 的 volumes 設定

7. 測試環境: Ubuntu-16.04
