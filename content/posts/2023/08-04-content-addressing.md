---
title: Content-addressable Storage vs Location-based Storage for Websites
description: Examples of content-addressable storage vs location-based
date: 2023-08-04
tags:
  - ipfs
author: Russell
---

Content-based storage allows the lookup of a website based on its contents instead of its name or location.

It works by passing the content through a hash function, which generates a content address.
This content address can then be used to request the content, and anyone who is hosting the content that hashes to that content address can serve the content.

### Location-Based Storage

Location-based storage is the traditional and widely used approach for accessing web content. It involves accessing data through known URLs that point to specific locations where the content is stored. Here are the key traits of location-based storage:

1. **Familiarity:** Most people are familiar with location-based storage, as it is the standard method for accessing websites and online content.
2. **Direct Retrieval:** Users access data by navigating to known URLs, which directly point to the server hosting the content.
3. **Hierarchical Structure:** Location-Based Storage organizes data in hierarchical structures, making it easy to manage and categorize content.
4. **Link Rot Susceptibility:** Over time, URLs might become invalid due to changes in the underlying infrastructure, leading to link rot.
5. **No built-in data verification:** location-based storage does not inherently verify the integrity of the data being accessed.

### Content-Addressable Storage

Content-addressable storage is a newer approach that ensures data integrity and immutability through unique content hashes. Instead of relying on URLs with specific locations, content is accessed based on its cryptographic hash. Let's explore the traits of content-addressable storage:

1. **Data Integrity and Immutability:** Each piece of content is identified by a unique content hash generated through a hash function. Any modification to the content results in a different hash, ensuring data integrity and immutability.
2. **Efficient Deduplication:** Since identical content generates the same content hash, content-addressable storage can efficiently deduplicate data, reducing storage overhead.
3. **Resilience to Link Rot:** Content is permanently accessible through its content hash, eliminating the issue of link rot experienced in location-based storage.
4. **Complexity vs. Direct Interaction:** While content hashes provide strong integrity, they may be less user-friendly for direct interaction compared to traditional URLs.
5. **Implementation Complexity:** Content-Addressable Storage may require slightly more complexity in implementation compared to straightforward location-based storage.

# Example: Location-Based Storage vs. Content-Addressable Storage

**Location-Based Storage Example:**

URL: [https://blog.dappling.network/](https://blog.dappling.network/)

In this blog, the URL "[https://blog.dappling.network/](https://blog.dappling.network/)" is a classic example of location-based storage. The content you're accessing is stored at the specific location, "blog.dappling.network." When you navigate to this URL, your web browser sends a request to the web server hosting the blog, and the server responds by delivering the content associated with that particular location. This is the conventional and familiar way of accessing web content, widely used for traditional websites and blogs.

**Content-Addressable Storage Example:**

URL: [https://bafybeigvbtiinlrfoewlccg4xgbxgsd4aa62gzjls6v2sg5aji4nu76n5q.ipfs.dweb.link/](https://bafybeigvbtiinlrfoewlccg4xgbxgsd4aa62gzjls6v2sg5aji4nu76n5q.ipfs.dweb.link/)

Now, let's consider a content-addressable storage example using IPFS (InterPlanetary File System) for this blog post. The URL "[https://bafybeigvbtiinlrfoewlccg4xgbxgsd4aa62gzjls6v2sg5aji4nu76n5q.ipfs.dweb.link/](https://bafybeigvbtiinlrfoewlccg4xgbxgsd4aa62gzjls6v2sg5aji4nu76n5q.ipfs.dweb.link/)" points to the content stored on the IPFS network. The string "bafybeigvbtiinlrfoewlccg4xgbxgsd4aa62gzjls6v2sg5aji4nu76n5q" is a unique content hash generated based on the blog post's data. When you access this URL, the IPFS network retrieves the content associated with that specific content hash, ensuring data integrity and immutability.

By using content-addressable storage, this blog post becomes permanently accessible via its unique content hash, even if the underlying IPFS network changes or moves.

# Conclusion

Both location-based storage and content-addressable storage present compelling use cases for hosting websites. Location-based storage offers a familiar and straightforward approach, enabling content retrieval through traditional URLs and hierarchical data organization.

On the other hand, content-addressable storage ensures strong data integrity and immutability by generating unique content hashes. This method promotes efficient deduplication, reduces storage overhead, and ensures resilience to link rot.

At [dAppling](https://dappling.network/landing), we are developing a platform that combines the strengths of both approaches to create a secure and performant hosting solution. By leveraging content addressing, we enhance data permanence and integrity while maintaining fast and efficient content retrieval for end-users. Our platform aims to empower developers with a user-centric and developer-friendly solution for building performant and resilient websites.
