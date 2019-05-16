# Queue logs

Queue logs are events logged by Asterisk in the queue\_log table of the
asterisk database. Queue logs are used to generate Wazo call center
statistics.

## Queue log sample

### Agent callback login

    time            |     callid      | queuename |   agent    |        event        |        data1        |      data2      |     data3     | data4 | data5

> \---------------------------+-----------------+-----------+------------+---------------------+---------------------+-----------------+---------------+-------+-------2012-07-03
> 15:27:23.896208 | 1341343640.4 | NONE | Agent/3001 |
> AGENTCALLBACKLOGIN | <1002@pcm-dev> | | | |

### Agent callback logoff

Agent/3001 is logged in queues q1 and
    q2.

    time            |     callid      | queuename |   agent    |        event        |        data1        |      data2      |     data3     | data4 | data5

> \---------------------------+-----------------+-----------+------------+---------------------+---------------------+-----------------+---------------+-------+-------2012-07-03
> 15:28:07.348244 | NONE | q2 | Agent/3001 | UNPAUSE | | | | |
> 2012-07-03 15:28:07.346320 | NONE | q1 | Agent/3001 | UNPAUSE | | | |
> | 2012-07-03 15:28:07.327425 | NONE | NONE | Agent/3001 | UNPAUSEALL |
> | | | | 2012-07-03 15:28:06.249357 | NONE | NONE | Agent/3001 |
> AGENTCALLBACKLOGOFF | <1002@pcm-dev> | 43 | CommandLogoff |
    |

### Call on a Queue with join empty conditions met

    time            |     callid      | queuename |   agent    |        event        |        data1        |      data2      |     data3     | data4 | data5

> \---------------------------+-----------------+-----------+------------+---------------------+---------------------+-----------------+---------------+-------+-------2012-07-04
> 07:27:55.640421 | 1341401275.9 | q1 | NONE | JOINEMPTY | | | |
    |

### Enter the queue and get answered by an agent

    time            |     callid      | queuename |   agent    |        event        |        data1        |      data2      |     data3     | data4 | data5

> \---------------------------+-----------------+-----------+------------+---------------------+---------------------+-----------------+---------------+-------+-------2012-07-04
> 07:33:23.085718 | 1341401601.24 | q1 | Agent/3001 | CONNECT | 2 |
> 1341401601.27 | 1 | | 2012-07-04 07:33:21.165823 | 1341401601.24 | q1
> | NONE | ENTERQUEUE | | 1000 | 1 |
    |

### Agent or caller ends the call after 12 seconds

    time            |     callid      | queuename |   agent    |        event        |        data1        |      data2      |     data3     | data4 | data5

> \---------------------------+-----------------+-----------+------------+---------------------+---------------------+-----------------+---------------+-------+-------2012-07-04
> 07:37:46.601754 | 1341401851.34 | q1 | Agent/3001 | COMPLETEAGENT | 2
> | 12 | 1 |
    |

### Call on a full queue

    time            |     callid      | queuename |   agent    |        event        |        data1        |      data2      |     data3     | data4 | data5

> \---------------------------+-----------------+-----------+------------+---------------------+---------------------+-----------------+---------------+-------+-------2012-07-04
> 07:40:17.339945 | 1341402016.44 | q1 | NONE | FULL | | | |
    |

### Call on a closed queue

    time            |     callid      | queuename |   agent    |        event        |        data1        |      data2      |     data3     | data4 | data5

> \---------------------------+-----------------+-----------+------------+---------------------+---------------------+-----------------+---------------+-------+-------2012-07-04
> 07:48:03.455999 | 1341402482.49 | q1 | NONE | CLOSED | | | |
    |

### Caller abandon before an answer

    time            |     callid      | queuename |   agent    |        event        |        data1        |      data2      |     data3     | data4 | data5

> \----------------------------+-----------------+-----------+------------+---------------------+---------------------+-----------------+---------------+-------+-------
> 2012-07-04 07:49:52.939802 | 1341402586.51 | q1 | NONE | ABANDON | 1 |
> 1 | 6 | |
