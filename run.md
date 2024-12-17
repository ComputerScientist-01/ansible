to run the ansible script

```
ansible-playbook -i inventory.ini download_from_nexus.yml --extra-vars "build=REGCORB3-1.11.5" -v
```
