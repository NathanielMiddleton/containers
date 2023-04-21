slurm_buildr
=========
Build a container with slurm dependencies already installed in an effort to create a standard environment

Process
-----------
Modify the Dockerfile as necessary
Build the image: ``docker build . -t local/slurm_builder:23.02``
docker run: ``docker run -v ./rpmbuild:/home/builder/rpmbuild -d local/slurm_builder:23.02``
Place spec file in ./rpmbuild/SPECS
Place slurm tar file in ./rpmbuild/SOURCES
connect to instance: ``docker exec -it container bash``
rpmbuild -ba slurm.spec --with slurmrestd --with lua --with hdf5 --with mysql
Distribute rpms from rpmbuilder/RPMS to repo
rebuild repo in the standard manner in the sysadmin wiki

Improvements
------------
This container could auto build RPMs with more time and effort
Un-hardcode versions to support multiple slurm versions

License
-------

Private

Author Information
------------------

Allen Institute ITO

