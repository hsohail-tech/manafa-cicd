# Use official Node.js image as a base
FROM node:14

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json into the container
COPY package*.json ./

# Install the dependencies
RUN npm install

# Copy the rest of the application code into the container
COPY . .

# Expose the application on port 3000
EXPOSE 3000

# Run the Nuxt.js application in development mode
CMD ["npm", "run", "dev"]

