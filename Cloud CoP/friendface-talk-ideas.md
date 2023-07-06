## Ideas for my FriendFace Cloud CoP talk

Overview

- Intro about me / background
  - New to software development and cloud technologies
  - Done Azure Fundamentals
- What is FriendFace
  - Grad training project - the first thing that you do as a graduate.
  - Social media application - mine wasn't very good, so I had many issues which I'll get into.
- Pre conceptions about deploying (IaaS)
  - Provisioning your own VM
  - Networking
  - Scary things
  - Talk about IaaS, PaaS and SaaS (there are more, but these are the main ones)
- Actuality (PaaS)
  - A lot of the bloat is done for you!
  - It's very user-friendly, and you only need to think about config things and code
  - PaaS is meant for developers!
  - All the tools that I used to deploy FriendFace are PaaS.
- Static Web app (HTML/CSS/JS)
  - For super simple applications which this was → literally was 1 weeks work when I knew nothing so very simple
  - Azure did a lot of the heavy lifting! Created a github actions build pipeline for me (some cheeky CICD built in)
  - Decided to create the app in the portal and then with the CD pipeline could make changes it would automatically redeploy.

Note: for the rest of them I didn't do a CICD pipeline because I've done a lot of work using Azure Devops (Did the pipelines for Retroflect) and didn't feel the need to implement that as (somewhat) I knew what I was doing.

- Azure web app (React/Java)
  - Had some mental barriers that were preconceptions that were wrong. Firstly for some reason I didn't build the API and then wondered why it wasn't working (very silly). Secondly the UI and API are separate web apps in their own right but I always thought of them as a package.
  - Didn't have a supported database (it was MariaDB and I had no idea how to work it) so changed to H2 in memory database (not hosted).
  - Got my UI web app and API web app up and running; it was happy days! Magic trick was restarting the web apps and that magically fixed problems of it not working.
  - I think because I was using free tier the compute resources (CPU/memory) were a bottleneck and making things not work properly. The app is **very slow**.
  - CORS problems - lots of the guides don't talk about this!

Note: Something I would have liked to do is set up CosmosDB database as I did it with another exercise and think It wouldn't be too difficult anymore but because at that moment in time I didn't know very much about CosmosDB thought it would be very confusing.

- Container apps (Also Container instances)

  - You can deploy in loads of different ways with containers (container apps, container instances, web apps, Azure Kubernetes and probably more)
  - The main idea is dockerise your UI/API, upload it to the Azure Container Registry (for version control among other things), then deploy it. I used Container apps which may have been a bit overkill but was told might as well!
  - I didn't get around to dockerising my FriendFace first time around so had to write the docker files (straight forward enough when consulting Retroflect and Summit Explorer as examples)
  - I did the deployment using the CLI which was nice (building and tagging docker image, pushing to ACR, deploying to Container Apps) I think it's a very similar process to deploy to container instances too.
  - I could provision more resources for my containers, so this implementation is much smoother! Originally my API container was crashing but when I gave it a bit more CPU and Memory and it was perfect.
  - Container Apps is set up to be good for larger scale applications over Azure web apps as it's easier to version control (with its good integration with ACR) and scale up/down to meet demand. Container Apps is serverless which in short means you're only charged when it's being used and when it's not being used it can scale to zero meaning it's off.

- Improvements
  - Web apps
    - Thing I would have liked to do/ maybe if the project was bigger and more people working on it, is set up slots → you can have a staging slot and production slot (environments) and when you want to release to production you just swap the staging and prod slots and hey presto you've deployed to prod!
    - You can also Implement SSO auth directly into Azure Web apps with all the different providers (Facebook, Google, Microsoft) which I could have looked into more but decided not to, it's not very customizable, but it should work in theory.
  - Container Apps
    - Could have implemented CI/CD to save to ACR and then publish to Container Apps but As I mentioned I didn't want to set up CI/CD as I'd done it already.
    - Could have implemented CosmosDB database and had that integration with a database rather than just the in memory one.
- Conclusion
