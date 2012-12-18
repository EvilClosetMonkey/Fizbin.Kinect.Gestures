Except on Tuesdays (Fizbin) Kinect Gesture Library
==================================================
http://www.exceptontuesdays.com/gestures-with-microsoft-kinect-for-windows-sdk-v1-5/

##Fizbin.Kinect.Gestures.Demo
Included in the repository is a simple demo application which shows how to easily set up a Kinect enabled application to recognize gestures.

###Requirements
The gesture demo takes advantage of helper classes included in the Developer Toolkit, which can be downloaded from the Microsoft Kinect for Windows homepage.
* `Microsoft.Kinect.Toolkit` – Used to reference to `KinectSensorChooser` component, for automatically finding and handling updates to the Kinect sensor.
* `Microsoft.Samples.Kinect.WpfViewers` – Used to reference the `KinectSensorManager` data model class.

For installing and using these packages you put them in parallel to the folder with this project solution.
You can install with "Developer Toolkit Browser" by installing the packages Microsoft.Kinect.Toolkit and "Kinect Explorer"
##Fizbin.Kinect.Gestures

###Executing on Gestures

In order to capture and execute on a gesture we initialize the gesture recognizer’s callback function in `InitializeKinectServices()`:

```csharp
gestureController = new GestureController();
gestureController.GestureRecognized += OnGestureRecognized;
```

The gesture recognizer analyzes data from the skeleton.  Each time we receive a new skeleton frame we send it off to the gesture recognizer for review:

```csharp
private void OnSkeletonFrameReady(object sender, SkeletonFrameReadyEventArgs e)
{
        using (SkeletonFrame frame = e.OpenSkeletonFrame())
        {
                if (frame == null)
                        return;

                // resize the skeletons array if needed
                if (skeletons.Length != frame.SkeletonArrayLength)
                        skeletons = new Skeleton[frame.SkeletonArrayLength];

                // get the skeleton data
                frame.CopySkeletonDataTo(skeletons);

                foreach (var skeleton in skeletons)
                {
                        // skip the skeleton if it is not being tracked
                        if (skeleton.TrackingState != SkeletonTrackingState.Tracked)
                                continue;

                        // update the gesture controller
                        gestureController.UpdateAllGestures(skeleton);
                }
        }
}
```

If a gesture is recognized an event will be fired.  We go to our event callback and execute on the type of gesture returned:

```csharp
private void OnGestureRecognized(object sender, GestureEventArgs e)
{
        Debug.WriteLine(e.GestureType);

        switch (e.GestureType)
        {
                case GestureType.Menu:
                        Gesture = "Menu";
                        break;
                case GestureType.WaveRight:
                        Gesture = "Wave Right";
                        break;
                case GestureType.WaveLeft:
                        Gesture = "Wave Left";
                        break;
                case GestureType.JoinedHands:
                        Gesture = "Joined Hands";
                        break;
                case GestureType.SwipeLeft:
                        Gesture = "Swipe Left";
                        break;
                case GestureType.SwipeRight:
                        Gesture = "Swipe Right";
                        break;
                case GestureType.ZoomIn:
                        Gesture = "Zoom In";
                        break;
                case GestureType.ZoomOut:
                        Gesture = "Zoom Out";
                        break;

                default:
                        break;
        }
}
```

That is all that is required.  Update any Kinect enabled application with the above lines, in addition to including the gesture library project, and you can execute on basic gestures!