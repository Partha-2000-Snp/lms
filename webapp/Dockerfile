# need node to build but to run we need nginx so we want to store the build operation done on node to a variable to use on nginx.
FROM node:16 AS buildfile

# same way we will create a folder to copy or code and use
RUN mkdir /frontend

# set working dir
WORKDIR /frontend

#copy code
COPY . .

# install npm and build the code
RUN npm install
RUN npm run build

# post build dist file will be generated. copy dist code which is stored on buildfile ref variable from node to nginx default path
FROM nginx

#copy to nginx
COPY --from=buildfile /frontend/dist /usr/share/nginx/html/
