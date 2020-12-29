from math import sqrt

import cv2


class PuRe():
    '''
        /*
     * 1) Canthi:
     * Using measurements from white men
     * Mean intercanthal distance 32.7 (2.4) mm
     * Mean palpebral fissure width 27.6 (1.9) mm
     * Jayanth Kunjur, T. Sabesan, V. Ilankovan
     * Anthropometric analysis of eyebrows and eyelids:
     * An inter-racial study
     */
    '''

    meanCanthiDistanceMM = 27.6
    # meanCanthiDistanceMM = 32.7
    '''
        /*
     * 2) Pupil:
     * 2 to 4 mm in diameter in bright light to 4 to 8 mm in the dark
     * Clinical Methods: The History, Physical, and Laboratory Examinations. 3rd
     * edition. Chapter 58The Pupils Robert H. Spector.
     */
    
    '''
    maxPupilDiameterMM = 8.0
    minPupilDiameterMM = 2.0
    '''
        /*
     * 3) Iris:
     *
     * Extremes of 10.2 to 13 mm
     * Caroline, P. J., and M. P. Andre.
     * "The effect of corneal diameter on soft lens fitting"
     * Contact Lens Spectrum 17.5 (2002): 56-56.
     *
     * Mean: 11.64 +/- 0.49 mm
     * Martin, Donald K., and Brien A. Holden.
     * "A new method for measuring the diameter of the in vivo human cornea."
     * American journal of optometry and physiological optics 59.5 (1982):
     * 436-441.
     */
    '''
    maxIrisDiameterMM = 13.0
    meanIrisDiameterMM = 11.64
    minIrisDiameterMM = 10.2

    def __init__(self):
        pass

    def estimateParameters(self, rows, cols):
        d = sqrt(rows ** 2 + cols ** 2)
        maxCanthiDistancePx = int(d)
        maxCanthiDistancePx = int(2.0 * d / 3.0)
        maxPupilDiameterPx = maxCanthiDistancePx * (maxPupilDiameterMM / meanCanthiDistanceMM)
        minPupilDiameterPx = minCanthiDistancePx * (minPupilDiameterMM / meanCanthiDistanceMM)

    def canny(self, input, blurImage, useL2, bins, nonEdgePixelsRatio, lowHighThresholdRatio):
        if blurImage:
            blurred = cv2.GaussianBlur(input, (5, 5), 1.5, 1.5, cv2.BORDER_REPLICATE)
        else:
            blurred = input
        dx = cv2.Sobel(blurred, cv2.CV_8UC3, 1, 0, 7, 1, 0, cv2.cv2.BORDER_REPLICATE)
        dy = cv2.Sobel(blurred, cv2.CV_8UC3, 0, 1, 7, 1, 0, cv2.cv2.BORDER_REPLICATE)

        magnitude = cv2.magnitude(dx, dy, )
        minMag, maxMag, minLoc, maxLoc = cv2.minMaxLoc(magnitude)


        magnitude = magnitude / maxMag

        res_idx = (bins - 1) * magnitude
        res_idx = cv2.cvtColor(res_idx,cv2.CV_16U)
        for i in range(res_idx.rows):
            p_res_idx = res_idx.ptr < short > (i);
        for (int j = 0; j < res_idx.cols; j++)
        histogram[p_res_idx[j]]++;
        }

        pass

    def detect_pupil(self, input):
        self.canny(input, True, True, 64, 0.7, 0.4)
