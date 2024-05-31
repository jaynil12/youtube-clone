# Stage 1: Build stage
FROM node:18 AS builder

# Setting the working directory in the container to /app
WORKDIR /app

# Copying package.json and package-lock.json into the working directory
COPY package*.json ./

# Installing any needed packages specified in package.json
RUN npm install

# Bundlling app source inside the docker image
COPY . .

# Building the app
RUN npm run build

# Stage 2: Production stage
FROM node:18

# Installing ffmpeg in the container
RUN apt-get update && apt-get install -y ffmpeg

# Setting the working directory
WORKDIR /app

# Copying package.json and package-lock.json
COPY package*.json ./

# Installing only production dependencies
RUN npm install --only=production

# Copying built app from the builder stage
COPY --from=builder /app/dist ./dist

# Make port 3000 available to the world outside this container
EXPOSE 3000

# Define the command to run your app using CMD which defines your runtime
CMD [ "npm", "run", "serve" ]
