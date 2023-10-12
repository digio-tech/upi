## Digio Android UPI SDK

![Github: release](https://img.shields.io/github/v/release/digio-tech/upi)
[![](https://jitpack.io/v/digio-tech/upi.svg)](https://jitpack.io/#digio-tech/upi)
[![License](https://img.shields.io/badge/License-Apache2.0-green.svg)](https://github.com/digio-tech/upi/blob/main/LICENSE)
[![Logo](https://img.shields.io/badge/Powered%20by-Digio-2979BF.svg)](https://www.digio.in)



### **How to Integrate?**

**Step 1.**
Add the JitPack.io repository to your build file. Add it in your root build.gradle(project) or settings.gradle.kts (project settings) at the end of repositories. Ignore incase already added to your project.

```
  dependencyResolutionManagement {
    ...
    repositories {
        ...
        maven(url = "https://jitpack.io")
    }
}
```


**Step 2.**
Add the digio upi dependency (build.gradle (module app)). Use latest version

	dependencies {
	        implementation ("com.github.digio-tech:upi:v1.0.0-beta02")
	}

**Step 3.**
Enable viewBinding and dataBinding in your build.gradle (module app)
```
android {
    ...
 buildFeatures {
        viewBinding = true
        dataBinding = true
    }
}    

```

**Step 4.**
Add below dependencies in build.gradle(Module:app). Ignore if already added


```
dependencies {
 
    implementation("androidx.core:core-ktx:1.9.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.9.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")
    
    // networking
    implementation ("com.squareup.retrofit2:retrofit:2.9.0")
    implementation ("com.squareup.okhttp3:okhttp:4.11.0")
    implementation ("com.squareup.okhttp3:logging-interceptor:4.8.0")
    implementation ("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation ("androidx.fragment:fragment-ktx:1.6.1")
    
    // navigation
    implementation ("androidx.navigation:navigation-fragment-ktx:2.6.0")
    implementation ("androidx.navigation:navigation-ui-ktx:2.6.0")
    implementation ("androidx.lifecycle:lifecycle-livedata-ktx:2.6.2")

    //firebase
    implementation("com.google.firebase:firebase-crashlytics-ktx:18.4.3")
    implementation("com.google.firebase:firebase-analytics-ktx:21.3.0")
}
```

**Step 5.**
Add plugins in build.gradle(module app). Ignore incase already added to your project

```
plugins {
    ...
    id("com.google.gms.google-services")
    id("com.google.firebase.crashlytics")
}
```

**Step 6.**

In your root-level (project-level) Gradle file (<project>/build.gradle.kts or <project>/build.gradle), add the Crashlytics and google service Gradle plugin to the plugins block. Ignore incase already add.

```
plugins {
    ...
    id("com.google.gms.google-services") version "4.3.15" apply false
    id ("com.google.firebase.crashlytics") version "2.9.9" apply false
}
```

**Step 7.**
Add digio Builder and pass required data.
```
 fun startDigioUpi(){
        try {
            val digio = Digio.Builder()
                .clientKeys("clientId","clientSecret") /** Mandatory Field**/
                .nachId("nachId") /** Mandatory:- Start with ENA..UP **/
                .environment(DigioEnvironment.PRODUCTION)
                .build()
            digio.start(mActivity) 
        }catch (e:Exception){
            Log.e("exception","${e.message}")
            e.printStackTrace()
        }
    }
```
**Step 8.**
Register DigioCallback to observe SUCCESS/FAILURE response. Observer should be in Activity/Fragment inside onCreate/onViewCreated

```
DigioCallback.register().transaction.observe(this, Observer {
            response -> when(response.status){
            DigioTransactionStatus.SUCCESS ->{
                val digioResponse = (response.data as DigioUpiResponse)
                Log.e("onTransaction","SUCCESS ${digioResponse.message}")
            }

            DigioTransactionStatus.FAILURE -> {
                val digioResponse = (response.data as DigioUpiResponse)
                Log.e("onTransaction","FAILURE ${digioResponse.message}")
            }
            else -> {}
            }
        })
```

**Step 9.**
Register digio event callback to observe events. Observer should be in Activity/Fragment inside onCreate/onViewCreated
```
DigioCallback.register().transactionEvents.observe(this,Observer{ data ->
            val digioEvents = (data as DigioEventResponse)
                Log.e("onTransaction","EVENTS ${digioEvents.event}")
        })
```

#### Parameter and type


| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `clientId` `clientSecret` | `string` | **Mandatory**. Your clientId and clientSecret key  |
| `nachId` | `string` | **Mandatory**. Your Nach Id |
| `PRODUCTION` `SANDBOX` | `DigioEnvironment` | **Mandatory**. Environment can be SANDBOX or PRODUCTION |


#### Error code and Description

| Code      | Description                |
| :-------- | :------------------------- |
| `1103`| **SUCCESS**  |
| `1104`| **FAILURE**  |
| `1105`| **CANCELLED**  |
| `1106`| **UPI APP NOT FOUND**  |

#### Response sample 

| Status    | Response                |
| :-------- | :------------------------- |
| `EVENTS`| DigioUPIEvents(action = DIGIO_INITIATED, screen = UPI_PAGE)  |
| `SUCCESS`| DigioUpiResponse(nachId= ENA23101017504882619RVM5Y1DTYPEUP, message= Mandate REGISTER_SUCCESS, statusCode= 1103, status= register_success )  |
| `FAILURE`| DigioUpiResponse(nachId= ENA23101017504882619RVM5Y1DTYPEUP, message= Transaction cancelled, statusCode= 1105, status= partial )  |



## License
[![License](https://img.shields.io/badge/License-Apache2.0-green.svg)](https://github.com/digio-tech/upi/blob/main/LICENSE)


## Support

For support, email support@digio.in




