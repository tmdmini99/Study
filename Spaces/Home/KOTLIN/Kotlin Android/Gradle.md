
기본 추가 gradle --아닐수도 있음


```
dependencies {
    implementation("com.google.code.gson:gson:2.8.8")
    implementation("androidx.core:core-ktx:1.13.1")
    implementation("androidx.appcompat:appcompat:1.7.0")
    implementation("com.google.android.material:material:1.12.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.8.4")
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.8.4")
    implementation("androidx.navigation:navigation-fragment-ktx:2.6.0")
    implementation("androidx.navigation:navigation-ui-ktx:2.6.0")

    // 코루틴 라이브러리 추가
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3")

    // OkHttp 라이브러리 추가 (네트워크 통신)
    implementation("com.squareup.okhttp3:okhttp:4.11.0")

    // Conscrypt 라이브러리 추가 (TLS/SSL 보안 라이브러리)
    implementation("org.conscrypt:conscrypt-android:2.5.2")

    // 테스트 종속성
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.ext:junit:1.2.1")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.6.1")
}

```