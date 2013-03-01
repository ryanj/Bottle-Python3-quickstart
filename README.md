Quick-start for Python 3.3 with Bottle framework running Mongo spatial queries
=============================

Running on OpenShift
----------------------------

Create an account at http://openshift.redhat.com/

Create a python-3.3 application and add a MongoDB cartridge to the app

    rhc app create <app name> python-3.3 mongodb-2.2

Add this quickstart repo

    cd <app name>
    git remote add quickstart git://github.com/openshift-quickstart/Bottle-Python3-quickstart.git
    git pull -s recursive -X theirs quickstart master
    
Then push the repo to OpenShift

    git push
    
To add the data to the MongoDB instance please follow the instructions on this blog:
[Mongo Spatial on OpenShift](https://openshift.redhat.com/community/blogs/spatial-mongodb-in-openshift-be-the-next-foursquare-part-1)

Now, ssh into the application.

    rhc ssh <app name>

Add the data to a collection called parkpoints:

    mongoimport -d parks -c parkpoints --type json --file $OPENSHIFT_REPO_DIR/wsgi/parkcoord.json -h $OPENSHIFT_MONGODB_DB_HOST -u $OPENSHIFT_MONGODB_DB_USERNAME -p $OPENSHIFT_MONGODB_DB_PASSWORD

Create the spatial index on the documents:

    mongo
    use parks
    db.parkpoints.ensureIndex( { pos : "2d" } );

Once the data is imported you can now checkout your application at:

    http://<app name>-<your namespace>.rhcloud.com/parks
