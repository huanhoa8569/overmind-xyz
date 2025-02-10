
# Overmind-XYZ

Overmind-XYZ is a Move module for a decentralized social media platform built on the Aptos blockchain. This platform allows users to create and manage accounts, follow other accounts, and post, comment, and like content. Account ownership is represented through NFTs.

## Features

### Account NFT Collection

-   Each user account is represented by an NFT.
    
-   The collection is called "account collection" with a trackable supply and no royalty fee.
    
-   Collection attributes:
    
    -   Name: "account collection"
        
    -   Description: "account collection description"
        
    -   URI: "account collection uri"
        

### Account NFT and Data

-   Each NFT represents a user account.
    
-   NFT attributes:
    
    -   Name: Username of the account
        
    -   Description: Empty (b"")
        
    -   URI: Empty (b"")
        
    -   No royalty fee
        
-   Each NFT stores two resources:
    
    -   `AccountMetaData`: Stores account-related metadata.
        
    -   `Publications`: Stores posts, comments, and likes.
        
-   Data limitations:
    
    -   Username: 1-32 characters
        
    -   Name: Max 60 characters
        
    -   Profile picture URI: Max 256 characters
        
    -   Bio: Max 160 characters
        
    -   Post and comment content: Max 280 characters
        

### Account Registry

-   The `AccountRegistry` struct maps registered usernames to NFT addresses.
    
-   Users can create new accounts if the username is not already taken.
    

### Publications

-   Three types of publications: **Posts**, **Comments**, and **Likes**.
    
-   **Posts**:
    
    -   Primary content type.
        
    -   Cannot be edited or deleted.
        
-   **Comments**:
    
    -   Associated with posts.
        
    -   Cannot be edited or deleted.
        
-   **Likes**:
    
    -   Can be applied to posts and comments.
        
    -   Users can only like once and can unlike if previously liked.
        

### References

-   Original publication data is stored in the creator's `Publications` resource.
    
-   Other users store references instead of duplicating data.
    
-   Example: If user A posts and user B comments, user B stores the comment, and a reference is added to user A's post.
    

### Global Timeline

-   The `GlobalTimeline` resource keeps references to all posts.
    
-   Stored in the module's resource account.
    

### Following System

-   Users can follow/unfollow each other.
    
-   Metadata updates to reflect new follows.
    

### View Functions

-   Allows querying platform data without modifying state.
    
-   Supports **pagination**:
    
    -   `page_size`: Number of items per page
        
    -   `page`: 0-indexed page number
        
-   **Error handling**: Returns empty values instead of aborting.
    
    -   Empty values:
        
        -   String: `string::utf8(b"")`
            
        -   Vectors: `vector[]`
            
        -   u64: `0`
            
        -   Address: `@0x0`
            
        -   Boolean: `false`
            

### Error Codes

Code

Description

`EUsernameAlreadyRegistered`

Username is already taken.

`EUsernameNotRegistered`

Action attempted on an unregistered username.

`EAccountDoesNotOwnUsername`

Unauthorized username action.

`EBioInvalidLength`

Bio length exceeds limit.

`EUserDoesNotFollowUser`

Unfollow attempt on a non-followed user.

`EUserFollowsUser`

Follow attempt on an already followed user.

`EPublicationDoesNotExistForUser`

Action attempted on a non-existent publication.

`EPublicationTypeToLikeIsInvalid`

Invalid publication type for liking.

`EUserHasLikedPublication`

Duplicate like attempt.

`EUserHasNotLikedPublication`

Unlike attempt without prior like.

`EUsersAreTheSame`

Attempt to follow/unfollow oneself.

`EInvalidPublicationType`

Action attempted on an invalid publication type.

`EUsernameInvalidLength`

Username length constraint violated.

`ENameInvalidLength`

Name length constraint violated.

`EProfilePictureUriInvalidLength`

Profile picture URI exceeds limit.

`EContentInvalidLength`

Post/comment content exceeds limit.
