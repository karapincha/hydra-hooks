# useViewport

This hook is used to get the current dimensions of the user's screen and determine if the screen is a mobile, tablet or desktop device. It returns an object with the current width and height of the screen, along with boolean values indicating whether the device is a mobile, tablet, or desktop device. It also updates the dimensions in real-time when the user resizes the screen.

`Dependent on **useMediaQuery** hook`

```
// Importing required hooks
import { useEffect, useState } from 'react'
import { useMediaQuery } from './useMediaQuery'

// Defining interfaces for the dimensions and device sizes
interface ViewportDimensions {
  width: number
  height: number
}

interface DeviceSizes {
  isMobileDevice: boolean
  isTabletDevice: boolean
  isDesktopDevice: boolean
}

// Defining the media queries for different device sizes
const MOBILE_QUERY = '(min-width: 1px) and (max-width: 767px)'
const TABLET_QUERY = '(min-width: 768px) and (max-width: 1199px)'
const DESKTOP_QUERY = '(min-width: 1200px)'

// Defining the hook for getting the viewport dimensions and device sizes
export const useViewport = (): [ViewportDimensions, DeviceSizes] => {
  // Initializing state for dimensions
  const [dimensions, setDimensions] = useState<ViewportDimensions>({
    height: window.innerHeight,
    width: window.innerWidth
  })

  // Using the useMediaQuery hook to determine device sizes
  const isMobileDevice = useMediaQuery(MOBILE_QUERY)
  const isTabletDevice = useMediaQuery(TABLET_QUERY)
  const isDesktopDevice = useMediaQuery(DESKTOP_QUERY)

  // Adding an event listener to window for real-time updates to dimensions
  useEffect(() => {
    const handleWindowResize = () => {
      setDimensions({
        height: window.innerHeight,
        width: window.innerWidth
      })
    }
    window.addEventListener('resize', handleWindowResize)
    return () => window.removeEventListener('resize', handleWindowResize)
  }, [])

  // Combining the device sizes into an object
  const deviceSizes: DeviceSizes = {
    isDesktopDevice,
    isMobileDevice,
    isTabletDevice
  }

  // Returning an array containing dimensions and device sizes
  return [dimensions, deviceSizes]
}

```
