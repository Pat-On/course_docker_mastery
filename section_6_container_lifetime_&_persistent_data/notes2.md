# Assignment: named Volumes

- database upgrade with container
- Create a postgres container with named volume `psql-data` using version 9.6.1
- Use Docker Hub to learn `VOLUME` path and versions needed to run it
- Check logs, stop container
- Create a new postrgres container with same named volume using 9.6.2
- Check logs to validate - it is going to have less logs, because it is going to re-use db
- (this only works with patch versions, most SQL DB's require manual commands to upgrade DB's to major/minor version, i.e. it's DB limitation not a container one)
