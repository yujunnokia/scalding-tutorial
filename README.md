# scalding-example-project

`scalding-example-project` is the Scalding [`WordCountJob`] [wordcount] example as a standalone project.

This was built by the [SnowPlow Analytics] [snowplow] team as a proof of concept for running Scalding on Amazon EMR.

## Building

Assuming you already have SBT installed:

    $ git clone git://github.com/snowplow/scalding-example-project.git
    $ cd scalding-example-project
    $ sbt
    scalding-example-project > assembly

The 'fat jar' is now available as:

    target/scalding-example-project-0.0.1.jar

## Running on Amazon EMR

First, upload the jar to S3 - if you haven't yet built the project (see above), you can grab the latest copy of the jar from [Downloads] [downloads].

Next, upload the data file `data/hello.txt` to S3.

Finally, you can run using the Ruby client:

    $ elastic-mapreduce --create --name "scalding-example-project" \
      --jar s3n://{{JAR_BUCKET}}/scalding-example-project-0.0.1.jar \
      --arg com.snowplowanalytics.hadoop.scalding.WordCountJob \
      --arg --hdfs \
      --arg --input --arg s3n://{{INPUT_BUCKET}}/hello.txt \
      --arg --output --arg s3n://{{OUTPUT_BUCKET}}/word-count.txt

Check the output once the run has completed.

## Troubleshooting

If you are trying to run this on a non-EMR environment, you may need to edit:

    project/BuildSettings.scala

And comment out the Hadoop jar exclusions:

    // "hadoop-core-0.20.2.jar", // Provided by Amazon EMR. Delete this line if you're not on EMR
    // "hadoop-tools-0.20.2.jar" // "

## Next steps

Fork this project and adapt it into your own custom Scalding job.

## Copyright and license

Copyright 2012 SnowPlow Analytics Ltd, with some portions copyright 2012 Twitter, Inc.

Licensed under the [Apache License, Version 2.0] [license] (the "License");
you may not use this software except in compliance with the License.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[wordcount]: https://github.com/twitter/scalding/blob/master/README.md
[snowplow]: http://snowplowanalytics.com
[downloads]: https://github.com/snowplow/scalding-example-project/downloads
[license]: http://www.apache.org/licenses/LICENSE-2.0