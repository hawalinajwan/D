<h1 align="center" style="font-size: 60px"> DRIVE </h1>

### Requirements
- NodeJS v16.x or Docker
- Postgres Database, Discord Webhook URLs

## Setup Guide
1. Clone this project
2. Create few webhook urls. For better performance and to avoid rate limit at least create 5 with 1 webhook / text channel. 
3. Setup postgres using docker, if you already don't have it running
   - `cd .devcontainer`
   - `docker-compose up -d`
4. Copy `config/.env_sample` to `config/.env` and make necessary changes
6. Run - `npm run migration:up`
7. Run - `node bin/ddrive`
8. Navigate to `http://localhost:3000` in your browser.

# Required params
DATABASE_URL= # Database URL of postgres with valid postgres uri

WEBHOOKS={url1},{url2} # Webhook urls seperated by ","

# Optional params
PORT=3000 # HTTP Port where ddrive panel will start running

REQUEST_TIMEOUT=60000 # Time in ms after which ddrive will abort request to discord api server. Set it high if you have very slow internet

CHUNK_SIZE=25165824 # ChunkSize in bytes. You should probably never touch this and if you do  don't set it to more than 25MB, with discord webhooks you can't upload file bigger than 25MB

SECRET=someverysecuresecret # If you set this every files on discord will be stored using strong encryption, but it will cause significantly high cpu usage, so don't use it unless you're storing important stuff

AUTH=admin:admin # Username password seperated by ":". If you set this panel will ask for username password before access

PUBLIC_ACCESS=READ_ONLY_FILE # If you want to give read only access to panel or file use this option. Check below for valid options.
                             # READ_ONLY_FILE - User will be only access download links of file and not panel
                             # READ_ONLY_PANEL - User will be able to browse the panel for files/directories but won't be able to upload/delete/rename any file/folder.

UPLOAD_CONCURRENCY=3 # ddrive will upload this many chunks in parallel to discord. If you have fast internet increasing it will significantly increase performance at cost of cpu/disk usage                                              

```

### Run using docker
```shell
docker run -rm -it -p 8080:8080 \
-e PORT=8080 \
-e WEBHOOKS={url1},{url2} \
-e DATABASE_URL={database url} \
--name ddrive forscht/ddrive
```



