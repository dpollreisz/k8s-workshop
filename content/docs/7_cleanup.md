# Cleanup

Now that we have an environment let’s clean it up and delete everything we just created.

First let’s delete our deployment

```
bosh -e aws -d nginx delete-deployment
```

After our deployment is gone let’s delete the BOSH Director itself. Copy your deploy.sh script to destroy.sh and change `create-env` to `delete-env` and run destroy.
```
cp deploy.sh destroy.sh
./destroy.sh
```

After the Bosh director is gone you can go ahead and delete the Jumpbox, and any other AWS resources you created!

You’ve completed the Bosh Workshop!

**Congratulations!**

