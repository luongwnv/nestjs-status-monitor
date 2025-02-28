# nestjs-status-monitor

[![NPM](https://nodei.co/npm/nestjs-status-monitor.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/nestjs-status-monitor/)

[![nestjs-status-monitor on npm](https://img.shields.io/npm/v/nestjs-status-monitor.svg)](https://www.npmjs.com/package/nestjs-status-monitor)
![David](https://img.shields.io/david/honnamkuan/nestjs-status-monitor)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/35443212eca84c2c94dd6dcfe4170ab3)](https://www.codacy.com/gh/honnamkuan/nestjs-status-monitor/dashboard?utm_source=github.com&utm_medium=referral&utm_content=honnamkuan/nestjs-status-monitor&utm_campaign=Badge_Grade)
![GitHub Workflow Status](https://img.shields.io/github/workflow/status/honnamkuan/nestjs-status-monitor/Node.js%20CI)
[![npm](https://img.shields.io/npm/dt/nestjs-status-monitor.svg)](https://img.shields.io/npm/dt/nestjs-status-monitor.svg)
![GitHub](https://img.shields.io/github/license/honnamkuan/nestjs-status-monitor)

````diff
- (Added socket.io.js into this package)
````

Simple, self-hosted module based on Socket.io and Chart.js to report realtime server metrics for NestJS v7+ based servers.

![Status monitor page](https://i.imgur.com/1xlO8lM.gif "Status monitor page")

## Live Demo

[Demo available here](https://nestjs-status-monitor.herokuapp.com/status)

## Installation & setup NestJS v7

1. Run `npm install nestjs-status-monitor --save`
2. Setup module import:

```javascript
@Module({
  imports: [StatusMonitorModule.forRoot()] //default config
})
```

3. Run server and visit `/status`

## Options

Monitor can be configured by passing options object during initialization of
module.

```javascript
@Module({
  imports: [StatusMonitorModule.forRoot(config)]
})
```

Default config:

```javascript
{
  title: 'NestJS Status', // Default title
  path: '/status',
  socketPath: '/socket.io', // In case you use a custom path
  port: null, // Defaults to NestJS port
  spans: [
    {
      interval: 1, // Every second
      retention: 60, // Keep 60 datapoints in memory
    },
    {
      interval: 5, // Every 5 seconds
      retention: 60,
    },
    {
      interval: 15, // Every 15 seconds
      retention: 60,
    },
  ],
  chartVisibility: {
    cpu: true,
    mem: true,
    load: true,
    eventLoop: true,
    heap: true,
    responseTime: true,
    rps: true,
    statusCodes: true,
  },
  ignoreStartsWith: ['/admin'], // paths to ignore for responseTime stats
  healthChecks: [],
}
```

## Health Checks

You can add a series of health checks to the configuration that will appear
below the other stats. The health check will be considered successful if the
endpoint returns a 200 status code.

```javascript
// config
healthChecks: [
  {
    protocol: 'http',
    host: 'localhost',
    path: '/health/alive',
    port: 3001,
  },
  {
    protocol: 'http',
    host: 'localhost',
    path: '/health/dead',
    port: 3001,
  },
];
```

## Local demo

1. Run the following:

```sh
npm i
cd examples/integrate-status-monitor
npm i
npm start
```

2. Visit [http://localhost:3001/status](http://localhost:3001/status)
