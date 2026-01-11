## What is DVC ??
Manage and version images, audio, video, and text files in storage and organize your ML modeling process into a reproducible workflow.

## Why DVC?
Even with all the success we've seen in machine learning, especially with deep learning and its applications in business, data scientists still lack best practices for organizing their projects and collaborating effectively. This is a critical challenge: while ML algorithms and methods are no longer tribal knowledge, they are still difficult to develop, reuse, and manage.

## Usecases:
If you store and process data files or datasets to produce other data or machine learning models, and you want to

- track and save data and machine learning models the same way you capture code;
- create and switch between versions of data and ML models easily;
- understand how datasets and ML artifacts were built in the first place;
- compare model metrics among experiments;
- adopt engineering tools and best practices in data science projects;

.gitignore -> we make a file by this name there we add venv/ -> i.e. this folder(venv (if created)) won't be tracked(whenever i do git init)

Note : DVC should work along with the git. We will use DVC along with git. DVC -> will only be used for tracking the data files , git -> will track other code information and configuration files required by DVC

git init to initialize a git repository

dvc init in terminal will initialize the dvc over here (after git init)

we will create a data folder , there we will create data.txt file and write this is the first version (dvc basically tracking version of the files, just like githb tracking recent commits or changes in the code files)

git status -> after git init then dvc init (this will show commited file) commited files include : .dvc/.gitignore , .dvc/config , .dvcignore -> all available in the .dvc folder

Commiting the files: git commit -m "dvc init" -> to commit all the files in .dvc folder

data.txt is the file u need to track : dvc add data/data.txt -> data is the folder and data.txt is the file in the folder When u are tracking the particular file the U icon is gone -> this means that particular file will now never be tracked with git it will be tracked with dvc hence gitignore will be created and that particular file name will be in gitignore -> this basically is giving indication to git that u don't have to track data.txt remaining can be tracked by git.

When u do the above command dvc add data/data.txt -> a new file will be created data.txt.dvc -> What is this ?? When u open that file its givng some kind of output: outs:

- md5: 368a315e36607be654bad6eae55b5865 -> This is bascially a hash key of the data.txt file or the particular file added(encrypted) size: 29 -> The hash key is created based on the content of data. This hash maps to a specific data hash: md5 -> The hash maps to the data in the particular file added(data.txt) path: data.txt

How is this mapping ?? If u change the data in data.txt and then add again , so what will happen -> the hash key will change Where do we find out mapping ?? when u go to .dvc folder -> their u will find out the folder named "cache" -> inside this cache folder u will be able to see that same hash key. And based on the hash key u have the content of the data.

Next command: git add data/data.txt.dvc -> Here i m tracking only this dvc file with git

git add data/.gitignore -> this was created after we tracked data.txt with dvc now we track this also with git

Once we do this icon A will appear in front of these files(i.e. it is in the added mode) git will track only these two files -> .gitignore and data.txt.dvc

git branch -> to know which branch it is -> currently its in master branch

Now we go and edit our data.txt file then we try to track it: dvc add data/data.txt

Now when u execute the above in terminal -> the hashkey in data.txt.dvc will change as we have altered the contents of 'data.txt' file Its changing bcoz hashkey is generated as per the content of the 'data.txt' file This new hashkey is also getting updated in cache. in .dvc -> cache -> we have multiple versions of hashkey generated for multiple versions of data(original , after update)

Then after this change again track ur 'data.txt.dvc' and '.gitignore' git add data/data.txt.dvc

Then, git status -> their we will see our new file

Then commit this specific file git commit -m "DVC"

Then to see all the logs: git log -> see the commits u have done at what time

How to move from one file to the other file ?? lets say i bring another change in my data.txt file this is my third version -> another text line added to my data.txt file

then do : dvc add data/data.txt -> again in .dvc we will get new folder(in cache in .dvc) -> new hashkey created(in data.txt.dvc)

then do git add data/data.txt.dvc Then commit it : git commit -m "This is the 3 version"

then git log -> to see the commits

How to see data at a particular commit ?? U will also be able to switch based on the data versioning.

git checkout be2b1db05daf4396d540c8d549a83813fb787e06 -> hashkey of previous commited data When u execute this , ur git will automatically switch to this particular commit Also ur hashkey in data.txt.dvc will change to be2b1db05daf4396d540c8d549a83813fb787e06 this hashkey as git was keeping track of data.txt.dvc and dvc was keeping track of data.txt , hence data.txt is not changed at all it has all the 3 statements added to it .

Now if u want to change the data.txt to the state where it was at the first commit i.e. commit id/hashkey = be2b1db05daf4396d540c8d549a83813fb787e06 then u have to do -> dvc checkout -> it will automatically link to the commit state(available rightnow) in data.txt.dvc and will change data.txt to that commit state also. After u execute this(dvc checkout) : data.txt will have only 2 lines which were available in earlier commit state. The 3rd comment will be gone.

To go back to ur original(most recent commit): git checkout master -> master branch is our most recent commit When u execute this it will change to our 'master' branch So now in data.txt.dvc u will see in md5 u have ur recent key

To get ur content back in data.txt file do : dvc checkout -> it will link with the commit state of data.txt.dvc and as that is recent it will also get the recent content. Hence after this the 3rd statement will be available in 'data.txt' file. -> we will get recent version of the data.

Note: checkout is used for switching