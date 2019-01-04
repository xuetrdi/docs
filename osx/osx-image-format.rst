============
ML on Iphone
============


Image format for Core ML
------------------------

- CVPixelBuffer

Image format for Vision
-----------------------

- CGImage
- CIImage(Core Image): process PNG and JPEG image files.
- Camera intrinsics
- automatically perform the conversion from the image colorspace to the model 


Vision
------

1. create VNCoreMLModel and VNCoreMLRequest instance.

   .. code-block:: swift
   
       let visionModel = try? VNCoreMLModel(for: coreMLmodel)
       let request = VNCoreMLRequest(model: visionModel) {
         request, error in /\* do sth with results \*/
       }
       request.imageCropAndScaleOption = .scaleFill

2. create a new VNImageRequestHandler instance tell it to perform the requests

   .. code-block:: swift
   
       let handler = VNImageRequestHandler(cgImage: yourImage)
       try? handler.perform([request])

3. the Vision result objects: VNObservation possibilities
   - Zero objects(no results)
   - VNClassificationObservation(classifier)
   - VNRecognizedObjectObservation(object detector)
   - VNCoreMLFeatureValueObservation(multiarray output)
   - VNPixelBufferObservation(style transfer)


Image cropping and scaling(VNCoreMLRequest.imageCropAndScaleOption property)
----------------------------------------------------------------------------

- centerCrop: scale-- fit shortest side, then rectangle around the image centor
- scaleFill: resize-- fit longest side, shortest side padded with zero, maintains aspect ratio
- scaleFit: withtout regard to the aspect ratio

Advandced pipelines
-------------------

- VNDetectFaceRectanglesRequest
- VNDetectRectanglesRequest
- VNTranslationalImageRegistrationRequest

Preprocessing
-------------

- RGB or BGR?

