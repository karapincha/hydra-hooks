# useMediaQuery

`useMediaQuery`, allows you to check whether a certain media query is currently matched by the user's device.


```
import { useEffect, useState } from 'react'

export function useMediaQuery(query: string): boolean {
  const [matches, setMatches] = useState<boolean>(false)
  const [isMatched, setIsMatched] = useState<boolean>(false)

  useEffect(() => {
    let media: MediaQueryList | null
    try {
      media = window.matchMedia(query)
    } catch (e) {
      console.error(`Failed to create media query: ${query}`, e)
      return
    }

    const handleMatches = () => {
      const newMatches = media?.matches // use optional chaining to access the matches property safely
      if (newMatches !== matches && newMatches != null) {
        setMatches(newMatches)
        setIsMatched(newMatches)
      }
    }

    try {
      handleMatches()
      media?.addEventListener('change', handleMatches) // use optional chaining to add the event listener safely
    } catch (e) {
      console.error(`Failed to add event listener to media query: ${query}`, e)
      return
    }

    return () => {
      try {
        media?.removeEventListener('change', handleMatches) // use optional chaining to remove the event listener safely
      } catch (e) {
        console.error(
          `Failed to remove event listener from media query: ${query}`,
          e
        )
      }
    }
  }, [query, matches])

  return isMatched
}
```
