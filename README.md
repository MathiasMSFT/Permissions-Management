# Permissions Management
Permissions Management is part of Microsoft Entra which includes Azure Active Directory and Verified ID


## Enable Permissions Management (video)

[![Alt text](https://img.youtube.com/vi/KiZny9ihP_s/0.jpg)](https://youtu.be/KiZny9ihP_s)



## Create a configuration (Azure)

[![Alt text](https://img.youtube.com/vi/5ru5M7aOvZk/0.jpg)](https://youtu.be/5ru5M7aOvZk)

### Change permission on your script
```
chmod +xr '<name of your script'
```

### Original script
```
#!/bin/bash -x

export AZURE_APP_ID=b46c3ac5-9da6-418f-a849-0a07a10b3c6c
export AZURE_SUBSCRIPTIONS=<ID of your subscription>
export AZURE_MANAGEMENT_GROUPS=<ID of your management group>

read -p "Enable controller. Enter 'y' or 'n'. More info at https://aka.ms/ciem-enable-controller: " MCIEM_ENABLE_AZURE_CONTROLLER
export MCIEM_ENABLE_AZURE_CONTROLLER=${MCIEM_ENABLE_AZURE_CONTROLLER:-n}

echo Adding role assignments to subscriptions $AZURE_SUBSCRIPTIONS with set controller flag to $MCIEM_ENABLE_AZURE_CONTROLLER

for AZURE_SUBSCRIPTION in $(echo $AZURE_SUBSCRIPTIONS | tr "," "\n"); do
  echo Adding Reader role assignment to subscriptions $AZURE_SUBSCRIPTION
  az role assignment create --assignee $AZURE_APP_ID --role "Reader" --subscription $AZURE_SUBSCRIPTION

  if [ $MCIEM_ENABLE_AZURE_CONTROLLER = 'y' ]; then
    echo Adding User Access Administrator role assignment to subscriptions $AZURE_SUBSCRIPTION
    az role assignment create --assignee $AZURE_APP_ID --role "User Access Administrator" --subscription $AZURE_SUBSCRIPTION
  fi
done

echo Adding role assignments to management groups $AZURE_MANAGEMENT_GROUPS with set controller flag to $MCIEM_ENABLE_AZURE_CONTROLLER
for AZURE_MANAGEMENT_GROUP in $(echo $AZURE_MANAGEMENT_GROUPS | tr "," "\n"); do
  echo Adding Reader role assignment to management group $AZURE_MANAGEMENT_GROUP
  az role assignment create --assignee $AZURE_APP_ID --role "Reader" --scope "/providers/Microsoft.Management/managementGroups/$AZURE_MANAGEMENT_GROUP"

  if [ $MCIEM_ENABLE_AZURE_CONTROLLER = 'y' ]; then
    echo Adding User Access Administrator role assignment to management group $AZURE_MANAGEMENT_GROUP
    az role assignment create --assignee $AZURE_APP_ID --role "User Access Administrator" --scope "/providers/Microsoft.Management/managementGroups/$AZURE_MANAGEMENT_GROUP"
  fi
done
```


## Delete a configuration

[![Alt text](https://img.youtube.com/vi/GgOSQtzwkc0/0.jpg)](https://youtu.be/GgOSQtzwkc0)



# Disclaimer
See [DISCLAIMER](./DISCLAIMER.md).