# need node 16 for building the app
FROM node:16

# need to copy the code
# if we simply copy the code basically code will be copied to root / . some default files well be there so we may get confeuse. 
# create own directory for code and other operations
RUN mkdir /backend

# set the created directory as default working dir
WORKDIR /backend

#copy the code from dir to node container dir
# my docker file is at same place where app code is so i will copy from current dir
# i have already set my working dir as /backend, so i will copy to current working dir in container.
COPY . .

# once the node is ready we need to install application dependency
RUN npm install

# create database schema and push table creation to postgres
RUN npx prisma generate
RUN npx prisma db push
RUN npm run build

# once our app build is ready we need to expose it. because by default node don't expose
EXPOSE 8080

# now everything is ready we need to start the application 
CMD ["node", "build/index.js"]
