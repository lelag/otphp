## Use

### Time based OTP's

    $totp = new OTPHP\TOTP("base32secret3232");
    $totp->now(); // => 492039

OTP verified for current time

    $totp->verify(492039); // => true

And 30s later

    $totp->verify(492039); // => false

### Counter based OTP's

    $hotp = new OTPHP\HOTP("base32secretkey3232");
    $hotp->at(0); // => 260182
    $hotp->at(1); // => 55283
    $hotp->at(1401); // => 316439

OTP verified with a counter

    $totp->verify(316439, 1401); // => true
    $totp->verify(316439, 1402); // => false

### Google Authenticator Compatible

The library works with the Google Authenticator iPhone and Android app, and also
includes the ability to generate provisioning URI's for use with the QR Code scanner
built into the app.

    $totp->provisioning_uri(); // => 'otpauth://totp/alice@google.com?secret=JBSWY3DPEHPK3PXP'
    $hotp->provisioning_uri(); // => 'otpauth://hotp/alice@google.com?secret=JBSWY3DPEHPK3PXP&counter=0'

This can then be rendered as a QR Code which can then be scanned and added to the users
list of OTP credentials.

#### Working example

Scan the following barcode with your phone, using Google Authenticator

![QR Code for OTP](http://chart.apis.google.com/chart?cht=qr&chs=250x250&chl=otpauth%3A%2F%2Ftotp%2Falice%40google.com%3Fsecret%3DJBSWY3DPEHPK3PXP)

Now run the following and compare the output

    $totp = new OTPHP\TOTP("JBSWY3DPEHPK3PXP");
    echo "Current OTP: ". $totp->now();
