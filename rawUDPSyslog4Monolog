class rawUDPSyslog4Monolog extends Monolog\Handler\AbstractProcessingHandler
{
    public $syslogConfigArray;
    private $initialized = false;
    private $sock;

    public function __construct($syslogConfigArray, $level = Logger::DEBUG, $bubble = true)
    {
        $this->syslogConfigArray = $syslogConfigArray;
        parent::__construct($level, $bubble);
    }

    private function initialize()
    {
        $this->sock = socket_create(AF_INET, SOCK_DGRAM, SOL_UDP);
        $this->initialized = true;
    }
    protected function write(array $record)
    {
        if (!$this->initialized) {
            $this->initialize();
        }
        $levelMonolog2RFC = array
        (
            100=>7,
            200=>6,
            250=>5,
            300=>4,
            400=>3,
            500=>2,
            550=>1,
            600=>0,
        );

        $prival = ($this->syslogConfigArray['facility']*8)+$levelMonolog2RFC [$record['level']];

        $msg = sprintf
        (
            "<%d>%s %s", 
            $prival, 
            $record['message'], 
            json_encode($record['context'], JSON_UNESCAPED_SLASHES|JSON_UNESCAPED_UNICODE)
        );
        
        socket_sendto($this->sock, $msg, strlen($msg), 0, $this->syslogConfigArray['host'],$this->syslogConfigArray['port']);
    }
}
