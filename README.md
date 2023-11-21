# Create eccommerce

## Auth

Authentication API Routes allow you to manage a customer's session, such as login or log out

### Get Current Customer

query:

```gql
query clientPortalCurrentUser {
  clientPortalCurrentUser {
    _id
    email
    firstName
    lastName
    type
    erxesCompanyId
    phone
    avatar
    customer {
      addresses
    }
    erxesCustomerId
    companyRegistrationNumber
  }
}
```

code:

```jsx
useQuery(queries.currentUser, {
  fetchPolicy: 'network-only',
  onCompleted(data) {
    const { clientPortalCurrentUser } = data || {};
    // logic
  },
});
```

Example: [https://github.com/pages-web/techstore/blob/main/src/modules/auth/currentUser.tsx]

### Customer Login

mutation:

```gql
mutation ClientPortalLogin(
  $clientPortalId: String!
  $login: String!
  $password: String!
) {
  clientPortalLogin(
    clientPortalId: $clientPortalId
    login: $login
    password: $password
  )
}
```

code:

```jsx
const [login, { loading }] = useMutation(mutations.login, {
  refetchQueries: [{ query: queries.currentUser }, 'clientPortalCurrentUser'],
  onError(error) {
    return toast.error(error.message);
  },
});
```

### Customer Logout

mutation:

```gql
const logout = gql`
 mutation {
   clientPortalLogout
 }
`;
```

code:

```jsx
const [login, { loading }] = useMutation(mutations.login, {
  refetchQueries: [{ query: queries.currentUser }, 'clientPortalCurrentUser'],
  onError(error) {
    return toast.error(error.message);
  },
});
```

Example: https://github.com/pages-web/techstore/blob/main/src/modules/auth/Login.tsx

# Products

### List Products

query:

```gql
query PoscProducts(
  $type: String
  $categoryId: String
  $searchValue: String
  $vendorId: String
  $tag: String
  $ids: [String]
  $excludeIds: Boolean
  $segment: String
  $segmentData: String
  $isKiosk: Boolean
  $groupedSimilarity: String
  $categoryMeta: String
  $page: Int
  $perPage: Int
  $sortField: String
  $sortDirection: Int
) {
  poscProducts(
    type: $type
    categoryId: $categoryId
    searchValue: $searchValue
    vendorId: $vendorId
    tag: $tag
    ids: $ids
    excludeIds: $excludeIds
    segment: $segment
    segmentData: $segmentData
    isKiosk: $isKiosk
    groupedSimilarity: $groupedSimilarity
    categoryMeta: $categoryMeta
    page: $page
    perPage: $perPage
    sortField: $sortField
    sortDirection: $sortDirection
  ) {
    _id
    name
    description
    attachment {
      url
      name
      type
      size
      duration
    }
    code
    shortName
    type
    barcodes
    barcodeDescription
    unitPrice
    categoryId
    customFieldsData
    customFieldsDataByFieldCode
    createdAt
    tagIds
    vendorId
    uom
    subUoms
    category {
      _id
      name
      description
      attachment {
        url
        name
        type
        size
        duration
      }
      code
      parentId
      meta
      order
      isRoot
      productCount
      maskType
      mask
      isSimilarity
      similarities
    }
    remainder
    soonIn
    soonOut
    remainders
    isCheckRem
    hasSimilarity
    attachmentMore {
      url
      name
      type
      size
      duration
    }
  }
}
```

Query parameters:

|                     |                                                                                             |
| ------------------- | ------------------------------------------------------------------------------------------- |
| `isKiosk`           | зэрэг посыг нь ашиглаж байгаа тохиолдолд зөвхөн пос дээр харагдах бараануудыг нуух          |
| `groupedSimilarity` | ижил төстэй бараануудыг багцалж харуулхад багцалсан төрлийг оруулана. `config` / `category` |

Example: https://github.com/erxes/erxes-community/blob/main/pos/modules/products/products.main.tsx (with infinite scroll)

### Product detail

query:

```gql
query PoscProductDetail($id: String, $branchId: String) {
  poscProductDetail(_id: $id, branchId: $branchId) {
    _id
    name
    description
    attachment {
      url
      name
      type
      size
      duration
    }
    code
    shortName
    type
    barcodes
    barcodeDescription
    unitPrice
    categoryId
    customFieldsData
    customFieldsDataByFieldCode
    createdAt
    tagIds
    vendorId
    attachmentMore {
      url
      name
      type
      size
      duration
    }
    uom
    subUoms
    category {
      _id
      name
      description
      attachment {
        url
        name
        type
        size
        duration
      }
      code
      parentId
      meta
      order
      isRoot
      productCount
      maskType
      mask
      isSimilarity
      similarities
    }
    remainder
    soonIn
    soonOut
    remainders
    isCheckRem
    hasSimilarity
  }
}
```

Query parameters:

|            |                             |
| ---------- | --------------------------- |
| `branchId` | Салбар дээрхи үлдэгдэл авах |

Example: https://github.com/pages-web/techstore/blob/main/src/lib/getProductDetail.tsx

### Product categories

query:

```gql
query PoscProductCategories(
  $parentId: String
  $withChild: Boolean
  $searchValue: String
  $status: String
  $excludeEmpty: Boolean
  $meta: String
  $isKiosk: Boolean
  $page: Int
  $perPage: Int
  $sortField: String
  $sortDirection: Int
) {
  poscProductCategories(
    parentId: $parentId
    withChild: $withChild
    searchValue: $searchValue
    status: $status
    excludeEmpty: $excludeEmpty
    meta: $meta
    isKiosk: $isKiosk
    page: $page
    perPage: $perPage
    sortField: $sortField
    sortDirection: $sortDirection
  ) {
    _id
    name
    description
    attachment {
      url
      name
      type
      size
      duration
    }
    code
    parentId
    meta
    order
    isRoot
    productCount
    maskType
    mask
    isSimilarity
    similarities
  }
}
```

Example: https://github.com/erxes/erxes/blob/master/pos/modules/products/components/CategoriesSheet.tsx (Мод байдлаар харуулсан дэлгэж хаах боломжтой)

### Product similarity

query:

```gql
query PoscProductSimilarities($id: String!, $groupedSimilarity: String) {
  poscProductSimilarities(_id: $id, groupedSimilarity: $groupedSimilarity) {
    groups {
      title
      fieldId
    }
    products {
      _id
      name
      description
      attachment {
        url
        name
        type
        size
        duration
      }
      code
      shortName
      type
      barcodes
      barcodeDescription
      unitPrice
      categoryId
      customFieldsData
      customFieldsDataByFieldCode
      createdAt
      tagIds
      vendorId
      attachmentMore {
        url
        name
        type
        size
        duration
      }
      uom
      subUoms
      category {
        _id
        name
        description
        attachment {
          url
          name
          type
          size
          duration
        }
        code
        parentId
        meta
        order
        isRoot
        productCount
        maskType
        mask
        isSimilarity
        similarities
      }
      remainder
      soonIn
      soonOut
      remainders
      isCheckRem
      hasSimilarity
    }
  }
}
```


