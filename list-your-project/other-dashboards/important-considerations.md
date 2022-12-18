# Important considerations

#### Timestamp

* All adapters should return last data from the \`timestamp\` time passed to the function. So in the case of the dimension \`dailyVolume\`, the adapter should return the volume of the last 24h from the \`timestamp\` provided (and not the volume of that day or the next 24h).

#### Methodology

* For fees and revenue adapters please don' forget to include how have you calculated the values in the methodology attribute

#### Incomplete data

* If the adapter is not able to provide data for all dimensions, let's say for example you can only provide data for \`dailyVolume\` but not for \`totalVolume\`, please keep the dimension as undefined, don't assign an arbitrary value or set to "0".
* If the adapter can't be used to backfill data and only allows to get data for today's timestamp, please set the flag \`runAtCurrTime\` to \`true\`. See more about this flag in the prevous section.

