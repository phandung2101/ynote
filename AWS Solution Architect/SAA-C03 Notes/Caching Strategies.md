# Some of this could be use

- CloudFront ( Cached static content or API that not frequently change )
- API Gateway ( Could have cache option )
- Redis/Memcached ( Logical caching or DB cache )
- DAX ( DynamoDB support )
- CloudFront Edge ( Cached at the edge to improve latency )

# Really depend on what is main problems

- Where do you want to cache?
- How long do you want to cache?
- ….

# Summary

Choose correct service for caching as your need