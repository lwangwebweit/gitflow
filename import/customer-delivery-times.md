# Customer delivery times

* ip\_customer\_delivery\_times table exists but it has only one example I think and to test this customer you need to update his customer\_number from 1673564 to 0001673564
* Depending on the code already implemented for importing files, It supposes we have zip files with the name customerdeliverytimes\*.zip in this path `/var/www/share/sharedmedia/import/acl/prod`, but right now there are no files\
  The implementation of code unzips these files and imports a list of files inside it
* The implemented code is supposed to import JSON format
* The command for importing customer delivery files already exists `interpneu:import:customerdeliverytimes`
* The implementation for handling customer delivery time is already done, but we might need some enhancement on the current code if this regarding this example
