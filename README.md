# Hướng dẫn sử dụng AdServer Module

- [Initialize](#initialize)
- [Quảng cáo native](#native)
- [Quảng cáo intersitial](#intersitial)
- [Quảng cáo open ad](#open-ad)

## Initialize 
Trong manifest<br>
```java
 <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="@string/your_admob_sdk_id" />
```

Set debug mode (trong SplashFragment)<br>
```kotlin
AdHolderOnline(activity).isDebugMode = true
```

## Native
__Bước 1: Tạo quảng cáo trong module adserver__
- Tạo layout quảng cáo native
- Vào package admob/ads 
- Trong hàm showAds ( [ADS_TEMPLATE] tự đặt )
```java
if (adsChild.getAdsTemplate().equals(AdDef.GOOGLE_AD_NATIVE_TEMPLATE.[ADS_TEMPLATE])) {
    adView = (UnifiedNativeAdView) activity.getLayoutInflater()
            .inflate([LAYOUT], null);
    populateUnifiedNativeAdView(unifiedNativeAd, adView, adsChild.getAdSize(), adsChild);
    frameLayout.removeAllViews();
    frameLayout.addView(adView);
    return;
}
```
__Bước 2: Khởi tạo list ad (Trong SplashFragment)__
```kotlin
private val adsHashMap = HashMap<String, Stack<AdsChild>>()
val [TenQuangCao] = Stack<AdsChild>()
[TenQuangCao].push(
      AdsChild(
          [ID QUẢNG CÁO],
          AdDef.NETWORK.GOOGLE,
          AdDef.GOOGLE_AD_TYPE.NEW_NATIVE,
          AdDef.GOOGLE_AD_NATIVE.NATIVE_LARGE,
          [ADS_TEMPLATE]
      )
  )
adsHashMap[TenQuangCao] = StoreNative
    
//Làm tương tự cho tất cả quảng cáo native, sau đó setListAd 
AdHolderOnline(activity).setListAd(adsHashMap)
```
__Bước 3: Show quảng cáo__
```kotlin
AdHolderOnline(activity).showAdsTotalOffline(
    [TenQuangCao],
    [LayoutQuangCao], 
    "",
    object : AdHolderOnline.AdHolderCallback {
        override fun onAdShow(network: String, adtype: String) {

        }

        override fun onAdClose(adType: String) {

        }

        override fun onAdFailToLoad(messageError: String) {

        }

        override fun onAdOff() {

        }
})
```

## Intersitial
```kotlin
AdmobInterstitialTest().showAdsTimeOut(
    activity,
    [ID Quảng Cáo],
    "", //Thêm text vào đây nếu muốn hiện dialog loading qc
    object : AdmobInterstitialTest.AdHolderCallback {
        override fun onAdShow(network: String, adtype: String) {
            //code here
        }

        override fun onAdClose(adType: String) {

        }

        override fun onAdFailToLoad(messageError: String) {
            //code here
        }

        override fun onAdOff() {

        }
    }, lifecycle, [timeOutInMillis]
)
```
Ở màn splash nếu muốn hiện thanh progress loading
```kotlin
 val splashProgress = SplashProgress(context, layout_ads)
 splashProgress.showProgress("#F8856A", "#F8856A", "#F8856A", "#A5A5A5")

AdmobInterstitialTest().showAdsTimeOut(
    activity,
    [ID Quảng Cáo],
    "", //Thêm text vào đây nếu muốn hiện dialog loading qc
    object : AdmobInterstitialTest.AdHolderCallback {
        override fun onAdShow(network: String, adtype: String) {
            //code here
            splashProgress.removeSplashProgress() //xóa thanh progress
            
        }

        override fun onAdClose(adType: String) {

        }

        override fun onAdFailToLoad(messageError: String) {
            //code here
        }

        override fun onAdOff() {

        }
    }, lifecycle, [timeOutInMillis]
)
```

### Open ad
