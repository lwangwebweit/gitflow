# known issue



```log
[2023-08-22T07:44:02.272207+00:00] interpneu_import.CRITICAL: The line item with position 20 was not found in order 400071549. [] []
```

```log
[2023-08-22T12:03:01.334604+00:00] interpneu_import.ERROR: ORDER-TRACKING-IMPORT-COMMAND: something went wrong on importing files {"exception-code":0,"exception-message":"SplFileInfo::getType(): Lstat failed for /var/www/share/interstore.pneu.com/shared/importFolder/interpneu_ordrsp_516559173_130.csv","exception-trace":"#0 /var/www/share/interstore.pneu.com/releases/70/vendor/league/flysystem/src/Adapter/Local.php(509): SplFileInfo->getType()\n#1 /var/www/share/interstore.pneu.com/releases/70/vendor/league/flysystem/src/Adapter/Local.php(452): League\Flysystem\Adapter\Local->mapFileInfo(Object(SplFileInfo))\n#2 /var/www/share/interstore.pneu.com/releases/70/vendor/league/flysystem/src/Adapter/Local.php(288): League\Flysystem\Adapter\Local->normalizeFileInfo(Object(SplFileInfo))\n#3 /var/www/share/interstore.pneu.com/releases/70/vendor/league/flysystem/src/Filesystem.php(272): League\Flysystem\Adapter\Local->listContents('', true)\n#4 /var/www/share/interstore.pneu.com/releases/70/custom/static-plugins/WebInterpneuCore/src/Import/Communication/FileReader/FileReader.php(24): League\Flysystem\Filesystem->listContents('', true)\n#5 /var/www/share/interstore.pneu.com/releases/70/custom/static-plugins/WebInterpneuB2B/src/Order/Import/Command/OrderTrackingImportCommand.php(56): Web\InterpneuCore\Import\Communication\FileReader\FileReader->fetchFiles('import', Object(Web\InterpneuCore\Import\Communication\FileReader\Filter\FileFilter))\n#6 /var/www/share/interstore.pneu.com/releases/70/vendor/symfony/console/Command/Command.php(298): Web\InterpneuB2B\Order\Import\Command\OrderTrackingImportCommand->execute(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))\n#7 /var/www/share/interstore.pneu.com/releases/70/vendor/symfony/console/Application.php(1058): Symfony\Component\Console\Command\Command->run(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))\n#8 /var/www/share/interstore.pneu.com/releases/70/vendor/symfony/framework-bundle/Console/Application.php(96): Symfony\Component\Console\Application->doRunCommand(Object(Web\InterpneuB2B\Order\Import\Command\OrderTrackingImportCommand), Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))\n#9 /var/www/share/interstore.pneu.com/releases/70/vendor/symfony/console/Application.php(301): Symfony\Bundle\FrameworkBundle\Console\Application->doRunCommand(Object(Web\InterpneuB2B\Order\Import\Command\OrderTrackingImportCommand), Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))\n#10 /var/www/share/interstore.pneu.com/releases/70/vendor/symfony/framework-bundle/Console/Application.php(82): Symfony\Component\Console\Application->doRun(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))\n#11 /var/www/share/interstore.pneu.com/releases/70/vendor/symfony/console/Application.php(171): Symfony\Bundle\FrameworkBundle\Console\Application->doRun(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))\n#12 /var/www/share/interstore.pneu.com/releases/70/bin/console(77): Symfony\Component\Console\Application->run(Object(Symfony\Component\Console\Input\ArgvInput))\n#13 {main}"} []
```