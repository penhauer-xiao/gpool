# GPool
A Goroutine pool for golang
```$xslt
package main

import (
	"fmt"
	"time"

	"github.com/penhauer-xiao/gpool"
)

var (
	download     gpool.TunnyWorker
	DownloadPool = gpool.NewPool(8, 50000, time.Duration(5))
)

type DownloadData struct {
	task string
}

type DownloadWorker struct {
}

func (worker *DownloadWorker) TunnyReady() bool {
	return true
}

// This is where the work actually happens
func (worker *DownloadWorker) TunnyJob(data interface{}) interface{} {
	in := data.(*DownloadData).task
	fmt.Println("download :", in)
	return nil
}

func main() {

	download = &DownloadWorker{}
	DownloadPool.Open(download)

	if !DownloadPool.SendJob(&DownloadData{task: "/video/xxx.mp4"}) {
		fmt.Println("The download pool is full, speed limit.")
	}
}

```