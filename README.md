# Photon API

Beginning with version 1.0.7, Photon offers an API allowing third-party tweaks to listen for Photon Ambient Display state changes.

**Header.h**
```objc
@interface PhotonAPI : NSObject
+ (BOOL)isShowing;
+ (BOOL)isInPocket;
@end
```

**Tweak.x(m)**
```objc
#import <notify.h> // Make sure to include the 'notification' header

- (void)yourInitMethod {
  ...
  
  // Register for 'Show Photon' notification
  int notify_token;
  notify_register_dispatch("com.cemck.photon/startAmbientDisplay", &notify_token, dispatch_get_main_queue(), ^(int token) {
      // Set your values animated
      [UIView animateWithDuration:0.9 delay:0 options:UIViewAnimationOptionCurveEaseInOut animations:^{
          self.view.alpha = 0; // Show/Hide your view
      } completion:nil];
  });
  
  // Register for 'Show Photon' notification
  int notify_token2;
  notify_register_dispatch("com.cemck.photon/startAmbientDisplay.noAnimation", &notify_token2, dispatch_get_main_queue(), ^(int token) {
      // Set your values
      self.view.alpha = 1; // Show/Hide your view
  });
  
  // Register for 'Hide Photon' notification
  int notify_token3;
  notify_register_dispatch("com.cemck.photon/stopAmbientDisplay", &notify_token3, dispatch_get_main_queue(), ^(int token) {
      // Set your values animated
      [UIView animateWithDuration:0.9 delay:0 options:UIViewAnimationOptionCurveEaseInOut animations:^{
          self.view.alpha = 1; // Show/Hide your view
      } completion:nil];
  });
}

- (void)yourAlternativeMethod {
  ...
  
  BOOL photonIsShowing = [%c(PhotonAPI) isShowing]; // Get the Ambient Display state
  
  if (photonIsShowing) {
    // Do stuff (Ambient Display is active)
  } else {
    // Do stuff (Ambient Display is hidden)
  }
}
```
