https://kafka.apache.org/25/documentation/streams/quickstart

	cd c:/kafka/bin/windows

Starting the Kafka server

	.. ON A SEPERATE COMMAND PROMPT
	zookeeper-server-start.bat config/zookeeper.properties 	

	.. ON A SEPERATE COMMAND PROMPT	
	kafka-server-start.bat config/server.properties

Prepare topics and describing them

	.. ON A SEPERATE COMMAND PROMPT
	kafka-topics.bat --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1--topic streams-plaintext-input
	
	kafka-topics.bat --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic streams-wordcount-output --config cleanup.policy=compact 
	
	kafka-topics.bat --bootstrap-server localhost:9092 --describe

Starting the WordCountDemo Application

	.. ON A SEPERATE COMMAND PROMPT
	
	kafka-run-class.bat org.apache.kafka.streams.examples.wordcount.WordCountDemo

..... The demo application will read from the input topic streams-plaintext-input, perform the computations of the
WordCount algorithm on each of the read messages, and continuously write its current results to the output <b>topic</b>
streams-wordcount-output.

	.. ON A SEPERATE COMMAND PROMPT
	
	kafka-console-producer.bat --bootstrap-server localhost:9092 --topic streams-plaintext-input
	
		-- Anmol 
		-- Sangram
		-- Anmol

.... These messages will be processed by the WordCount Application and the following output data will be written to the
streams-wordcount-output topic and printed by the console consumer:

	kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic streams-wordcount-output --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property print.value=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer 
		-- Anmol 1
		-- Sangram 1
		-- Anmol 2

.... the first column is the Kafka message key in java.lang.String format and represents a word that is being counted,
and the second column is the message value in java.lang.Longformat, representing the word's latest count.