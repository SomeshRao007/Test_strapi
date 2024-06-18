# Test_strapi

Update and Install Dependencies


sudo apt update


Install Node.js and npm:

curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

sudo apt install -y nodejs


Verify installation:

node -v
npm -v


Install PostgreSQL (optional):


sudo apt install -y postgresql postgresql-contrib


Start and enable PostgreSQL service:

sudo systemctl start postgresql
sudo systemctl enable postgresql


Install Yarn:


npm install -g yarn


Run the installation script and create a Strapi Cloud account

npx create-strapi-app@latest my-strapi-project --quickstart

The terminal will invite you to create a Strapi Cloud account and start a free, 14-day trial. Ensure Login/Sign up is selected in the terminal, or use arrow keys to select it, and press Enter.
