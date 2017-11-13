# Server Config

Configuration of Flagr server is derived from the environment variables. Latest [env.go](https://github.com/checkr/flagr/blob/master/pkg/config/env.go).

```go
// filepath: pkg/config/env.go
var Config = struct {
	// Host - Flagr server host
	Host string `env:"HOST" envDefault:"localhost"`
	// Port - Flagr server port
	Port int `env:"PORT" envDefault:"18000"`

	// GracefulCleanupTimeout - the timeout after graceful shutdown. It's useful to mitigate Kubernetes
	// rolling update race condition.
	GracefulCleanupTimeout time.Duration `env:"FLAGR_GRACEFUL_CLEANUP_TIMEOUT" envDefault:"2s"`

	// PProfEnabled - to enable the standard pprof of golang's http server
	PProfEnabled bool `env:"FLAGR_PPROF_ENABLED" envDefault:"true"`

	// DBDriver - Flagr supports sqlite3, mysql, postgres
	DBDriver string `env:"FLAGR_DB_DBDRIVER" envDefault:"sqlite3"`
	// DBConnectionStr - examples
	// sqlite3:  "/tmp/file.db"
	// sqlite3:  ":memory:"
	// mysql:    "root:@tcp(127.0.0.1:18100)/flagr?parseTime=true"
	// postgres: "host=myhost user=root dbname=flagr password=mypassword"
	DBConnectionStr string `env:"FLAGR_DB_DBCONNECTIONSTR" envDefault:"flagr.sqlite"`

	// CORSEnabled - enable CORS
	CORSEnabled bool `env:"FLAGR_CORS_ENABLED" envDefault:"true"`

	// SentryEnabled - enable Sentry
	SentryEnabled bool `env:"FLAGR_SENTRY_ENABLED" envDefault:"false"`

	// SentryDSN - sentry DSN
	SentryDSN string `env:"FLAGR_SENTRY_DSN" envDefault:""`

	// NewRelicEnabled - enable the NewRelic monitoring for all the endpoints and DB operations
	NewRelicEnabled bool   `env:"FLAGR_NEWRELIC_ENABLED" envDefault:"false"`
	NewRelicAppName string `env:"FLAGR_NEWRELIC_NAME" envDefault:"flagr"`
	NewRelicKey     string `env:"FLAGR_NEWRELIC_KEY" envDefault:""`

	// EvalCacheRefreshTimeout - timeout of getting the flags data from DB into the in-memory evaluation cache
	EvalCacheRefreshTimeout time.Duration `env:"FLAGR_EVALCACHE_REFRESHTIMEOUT" envDefault:"59s"`
	// EvalCacheRefreshInterval - time interval of getting the flags data from DB into the in-memory evaluation cache
	EvalCacheRefreshInterval time.Duration `env:"FLAGR_EVALCACHE_REFRESHINTERVAL" envDefault:"3s"`

	// RecorderEnabled - enable data records logging
	RecorderEnabled bool `env:"FLAGR_RECORDER_ENABLED" envDefault:"false"`
	// RecorderType - the pipeline to log data records, e.g. Kafka
	RecorderType string `env:"FLAGR_RECORDER_TYPE" envDefault:"kafka"`

	// RecorderKafkaBrokers and etc. - Kafka related configurations for data records logging (Flagr Metrics)
	RecorderKafkaBrokers        string        `env:"FLAGR_RECORDER_KAFKA_BROKERS" envDefault:":9092"`
	RecorderKafkaCertFile       string        `env:"FLAGR_RECORDER_KAFKA_CERTFILE" envDefault:""`
	RecorderKafkaKeyFile        string        `env:"FLAGR_RECORDER_KAFKA_KEYFILE" envDefault:""`
	RecorderKafkaCAFile         string        `env:"FLAGR_RECORDER_KAFKA_CAFILE" envDefault:""`
	RecorderKafkaVerifySSL      bool          `env:"FLAGR_RECORDER_KAFKA_VERIFYSSL" envDefault:"false"`
	RecorderKafkaVerbose        bool          `env:"FLAGR_RECORDER_KAFKA_VERBOSE" envDefault:"true"`
	RecorderKafkaTopic          string        `env:"FLAGR_RECORDER_KAFKA_TOPIC" envDefault:"flagr-records"`
	RecorderKafkaRetryMax       int           `env:"FLAGR_RECORDER_KAFKA_RETRYMAX" envDefault:"5"`
	RecorderKafkaFlushFrequency time.Duration `env:"FLAGR_RECORDER_KAFKA_FLUSHFREQUENCY" envDefault:"500ms"`
	RecorderKafkaEncrypted      bool          `env:"FLAGR_RECORDER_KAFKA_ENCRYPTED" envDefault:"false"`
	RecorderKafkaEncryptionKey  string        `env:"FLAGR_RECORDER_KAFKA_ENCRYPTION_KEY" envDefault:""`
}{}
```

For example

```go
// setting env variable FLAGR_DB_DBDRIVER=mysql results in
Config.DBDriver = "mysql"
```