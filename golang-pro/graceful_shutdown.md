**Start server in a go routine**
```
go func() {
	fmt.Println("Starting server on port", port)
	if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
		log.Fatalf("Failed to start server: %v", err)
	}
}()
```

**Register a channel to hear the OS signal**
Create channel -> Register to listen signal -> Wait OS signal
```
quit := make(chan os.Signal, 1)
signal.Notify(quit, os.Interrupt, syscall.SIGTERM)
<-quit
```

**Graceful shutdown with timeout**
Timeout if shutdown step spend > 30s
```
ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
defer cancel()

if err := server.Shutdown(ctx); err != nil {
  log.Fatalf("Failed to shutdown server: %v", err)
}
```
