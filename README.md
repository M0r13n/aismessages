![Build status](https://travis-ci.org/tbsalling/aismessages.svg?branch=master)
[![License](http://img.shields.io/badge/license-CCANS3-green.svg)](https://github.com/tbsalling/aismessages/blob/master/LICENSE)

Introduction
---
AISmessages is a Java-based light-weight, zero-dependency, and ultra-efficient message decoder for maritime
navigation and safety messages compliant with ITU 1371 (NMEA armoured AIS messages).

It exhibits lazy decoding and fully decodes tens of thousands of NMEA armoured AIS messages per second per CPU
core on an Intel i7-based laptop.

Applications, demos, and talks
---
See simple demo applications in the source code in the `dk.tbsalling.aismessages.demo` package (view via 
[Github](https://github.com/tbsalling/aismessages/tree/master/src/main/java/dk/tbsalling/aismessages/demo)). 

A live demo of AISmessages is available on http://ais.tbsalling.dk. 

See also how AISmessages was utilized by OpenRemote, Inc. for the Safe Waterways project in the Beatrix Canal, NL, in this [Youtube video](https://youtu.be/_pcH0KB5J2Q):

<iframe width="560" height="315" src="https://www.youtube.com/embed/_pcH0KB5J2Q" frameborder="0"></iframe>

AISmessages has also been addressed in talks at various conferences; e.g. [at DEVOXX BE 2015](http://cfp.devoxx.be/2015/talk/OOF-1013/Decoding_the_air_around_you_with_Java_and_$7_hardware) by Bert Jan Schrijver of JPoint ([@bjschrijver](https://twitter.com/bjschrijver)):  

Programmatic usage
---
Programmatically the starting point is the AISStreamReader. It takes an InputStream (feeding
NMEA data), and a consumer of AISMessages which as called back every time an AIS message is decoded. So,
if you have an InputStream serving data like this:

```
    !AIVDM,1,1,,A,15Mv5v?P00IS0J`A86KTROvN0<5k,0*12
    !AIVDM,1,1,,A,15Mwd<PP00ISfGpA7jBr??vP0<3:,0*04
    !AIVDM,2,1,4,B,55MwW7P00001L@?;GS0<51B08Thj0TdpE800000P0hD556IE07RlSm6P0000,0*0B
    !AIVDM,2,2,4,B,00000000000,2*23
    !AIVDM,1,1,,A,15N7th0P00ISsi4A5I?:fgvP2<40,0*06
    !AIVDM,1,1,,A,15NIEcP000ISrjPA8tEIBq<P089=,0*63
    !AIVDM,1,1,,B,15MuS0PP00IS00HA8gEtSgvN0<3U,0*61
```

then you can decode it into Java POJO's of type AISMessage like this:

``` java

    public class DemoApp {

        public static void main(String[] args) throws IOException {

           InputStream inputStream = ...

           AISInputStreamReader streamReader
                = new AISInputStreamReader(
                    inputStream,
                    aisMessage -> System.out.println(aisMessage))
           );

           streamReader.run();
	    }

    }
```

Read more in the wiki - [https://github.com/tbsalling/aismessages/wiki](https://github.com/tbsalling/aismessages/wiki).

You do not need to compile AISmessages yourself. It is available in Maven Central. So if you are using Maven, 
all you need to do is add these lines to your pom.xml:

``` xml
	<dependency>
	    <groupId>dk.tbsalling</groupId>
	    <artifactId>aismessages</artifactId>
	    <version>2.1.0</version>
	</dependency>
```

If you need more advanced funtionality than merely decoding AIS messages, then you might also want to have a look at 
AISutils: https://github.com/tbsalling/aisutils.

---

**Important compiler note!** Due to a [compiler bug](https://bugs.openjdk.java.net/browse/JDK-8062253) in JDK 8u20, 8u25, and 8u31
AISmessages can only be compiled with older or newer JDK 8 versions than these faulty ones.

---
