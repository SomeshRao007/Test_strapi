# 🚀 Getting started with Strapi

Strapi comes with a full featured [Command Line Interface](https://docs.strapi.io/dev-docs/cli) (CLI) which lets you scaffold and manage your project in seconds.

INstallation on EC2 Ubuntu : 


Update and Install Dependencies

~~~
sudo apt update
~~~


Install Node.js and npm:

~~~
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

sudo apt install -y nodejs
~~~

Verify installation:

~~~
node -v
npm -v
~~~

Install PostgreSQL (optional):

~~~
sudo apt install -y postgresql postgresql-contrib
~~~

Start and enable PostgreSQL service:

~~~
sudo systemctl start postgresql
sudo systemctl enable postgresql
~~~

Install Yarn:

~~~
npm install -g yarn
~~~

Run the installation script and create a Strapi Cloud account

~~~
npx create-strapi-app@latest my-strapi-project --quickstart
~~~


The terminal will invite you to create a Strapi Cloud account and start a free, 14-day trial. Ensure Login/Sign up is selected in the terminal, or use arrow keys to select it, and press Enter.


### `develop`

Start your Strapi application with autoReload enabled. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-develop)

```
npm run develop
# or
yarn develop
```

### `start`

Start your Strapi application with autoReload disabled. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-start)

```
npm run start
# or
yarn start
```

### `build`

Build your admin panel. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-build)

```
npm run build
# or
yarn build
```

## ⚙️ Deployment

Strapi gives you many possible deployment options for your project including [Strapi Cloud](https://cloud.strapi.io). Browse the [deployment section of the documentation](https://docs.strapi.io/dev-docs/deployment) to find the best solution for your use case.


![Screenshot 2024-06-18 184708](https://github.com/SomeshRao007/Test_strapi/assets/111784343/6c50ef64-915d-4034-9697-fb9d83adb6c1)


Setup POSTGRESS DB:

Update package lists

~~~
sudo apt update
~~~

Install PostgreSQL and contrib package:

~~~
sudo apt install -y postgresql postgresql-contrib
~~~

Switch to the PostgreSQL user and create the database and user:

~~~
sudo -i -u postgres <<EOF
psql -c "CREATE DATABASE strapi_database;"
psql -c "CREATE USER strapiuser WITH PASSWORD 'strapipassword';"
psql -c "GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;"
EOF
~~~

Verify the creation of database and user:

~~~
sudo -i -u postgres psql -c "\l"
sudo -i -u postgres psql -c "\du"
~~~

![Screenshot 2024-06-18 185251](https://github.com/SomeshRao007/Test_strapi/assets/111784343/9ca77651-e3b0-4794-8e30-f0d65ad2f90f)


Since, you are using PostgreSQL, update the database configuration in config/database.js:

~~~
module.exports = ({ env }) => ({
  defaultConnection: 'default',
  connections: {
    default: {
      connector: 'bookshelf',
      settings: {
        client: 'postgres',
        host: env('DATABASE_HOST', '127.0.0.1'),
        port: env('DATABASE_PORT', 5432),
        database: env('DATABASE_NAME', 'strapi'),
        username: env('DATABASE_USERNAME', 'strapi'),
        password: env('DATABASE_PASSWORD', 'strapi'),
        ssl: env.bool('DATABASE_SSL', false),
      },
      options: {},
    },
  },
});
~~~

UPDATE THE VALUES WITH YOUR VALUES.

> Reboot your EC2 instance

Start Strapi:

~~~
yarn develop
~~~

If you want Strapi to run in the background, you can use a process manager like PM2:

~~~
npm install -g pm2 
pm2 start npm --name "strapi" -- run start
pm2 startup
pm2 save
~~~


![Screenshot 2024-06-18 191427](https://github.com/SomeshRao007/Test_strapi/assets/111784343/83322d4a-e352-429b-81c7-65092deec35e)


**GITHUB CICD FILE**

~~~
# This is a basic workflow to help you get started with Actions

name: Deploy Strapi

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "master" branch
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]

jobs:
   build-and-deploy:
       environment: DEV
       runs-on: ubuntu-latest

       steps:
        - name: Checkout code
          uses: actions/checkout@v2

        - name: Set up Node.js
          uses: actions/setup-node@v2
          with:
            node-version: '20.x'

        - name: Install dependencies
        
          run: npm install -g yarn



        - name: Prepare SSH Key
          run: |
            echo "${{ secrets.SSH_PRIVATE_KEY }}" > key.pem
            chmod 600 key.pem
        - name: Deploy to server
          env:
            HOST: ${{ secrets.HOST }}
            USERNAME: ${{ secrets.USERNAME }}
            SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}
            ENV_VARS: ${{ secrets.ENV_VARS }}
          
          run: |
            
            ssh -i key.pem -o StrictHostKeyChecking=no ${USERNAME}@${HOST} '
            cd ~/my-strapi-project &&
            git pull origin master &&
            yarn install &&
            yarn build &&
            pm2 restart strapi
            '
~~~


![Uploading Screenshot 2024-06-18 202512.png…]()



## 📚 Learn more

- [Resource center](https://strapi.io/resource-center) - Strapi resource center.
- [Strapi documentation](https://docs.strapi.io) - Official Strapi documentation.
- [Strapi tutorials](https://strapi.io/tutorials) - List of tutorials made by the core team and the community.
- [Strapi blog](https://strapi.io/blog) - Official Strapi blog containing articles made by the Strapi team and the community.
- [Changelog](https://strapi.io/changelog) - Find out about the Strapi product updates, new features and general improvements.

Feel free to check out the [Strapi GitHub repository](https://github.com/strapi/strapi). Your feedback and contributions are welcome!

## ✨ Community

- [Discord](https://discord.strapi.io) - Come chat with the Strapi community including the core team.
- [Forum](https://forum.strapi.io/) - Place to discuss, ask questions and find answers, show your Strapi project and get feedback or just talk with other Community members.
- [Awesome Strapi](https://github.com/strapi/awesome-strapi) - A curated list of awesome things related to Strapi.

---

<sub>🤫 Psst! [Strapi is hiring](https://strapi.io/careers).</sub>
