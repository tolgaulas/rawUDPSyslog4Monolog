# rawUDPSyslog4Monolog
non-PSR3 handler to send raw message to a syslog server over UDP

example:

$syslogConfigArray = array
(
    'host'=>'localhost',
    'port'=>514,
    'facility'=>16 //local0
);

$syslog_handler = new mySyslogHandler($syslogConfigArray,\Monolog\Logger::DEBUG, true);

$logger = new \Monolog\Logger('testus');

$logger->pushHandler($syslog_handler);

$logger->info('Something interesting happened');
