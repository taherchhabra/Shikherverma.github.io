---
published: true
layout: post
subtitle: Specifying your business rule at one single place
date: 2011-08-23T12:23:33.000Z
author: Taher Chhabra
header-img: img/posts/spec.jpg
comments: true
tags:
  - Design Pattern
---
Suppose there is a method as shown below

 

    public List GetExpensiveProducts()
    {
        List products = GetAllProducts();
        List expensiveProducts = new List();
        foreach (product p in products)
        {
            if(product.price > 50)
            {
                 expensiveProducts.add(p)
            }
        }
        return expensiveProducts;
    }

It is a method which returns all expensive products and there are other 10 methods like

    GetExpensiveProductsInStock()
    GetExpensiveProductsOutOfStock()
    GetExpensiveProductInACategory() etc etc etc ..

now suddenly your business analyst says that “we need to change the definition of expensive products, now products having price higher than 100 are expensive products”

So you’ll need to change the code at 10 different places ..isnt it?

The Specification pattern comes to the rescue !!

Now you can write the above method like this

 

    public List GetExpensiveProducts()
    {
        List products = GetAllProducts();
        List expensiveProducts = new List();
        ExpensiveProductSpecification spec = new ExpensiveProductSpecification ()
        foreach (product p in products)
        {
             if(spec.IsSatisfiedBy(p))
             {
                   expensiveProducts.add(p)
             }
         }
         return expensiveProducts;
    }


The abstract base class of the pattern is as follows


    public abstract class Specification
    {
         public abstract bool IsSatisfiedBy(T obj);
    }

to make a concrete specification, we just need to inherit from the above base class

    public class ProductPriceGreaterThanSpecification : Specification
    {
         public override bool IsSatisfiedBy(Product product)
         {
              return product.Price > 50
         }
    }


We can also create composite specifications, For that we’ll need to create a generic Class named CompositeSpecification by deriving the base class as shown below


    public abstract class CompositeSpecification : Specification
    {
           protected readonly Specification _leftSide;
           protected readonly Specification _rightSide;

           public CompositeSpecification(Specification leftSide, Specification rightSide)
           {
                 _leftSide = leftSide;
                 _rightSide = rightSide;
            }
    }



now lets create concrete implementations of the CompositeSpecification

    public class AndSpecification : CompositeSpecification
    {

            public AndSpecification(Specification leftSide, Specification rightSide): base(leftSide, rightSide)
            {

            }

            public override bool IsSatisfiedBy(T obj)
            {
                return _leftSide.IsSatisfiedBy(obj) && _rightSide.IsSatisfiedBy(obj);
            }
    }





    public class OrSpecification : CompositeSpecification
    {

             public OrSpecification(Specification leftSide, Specification rightSide): base(leftSide, rightSide)
             {

             }

             public override bool IsSatisfiedBy(T obj)
             {
                  return _leftSide.IsSatisfiedBy(obj) || _rightSide.IsSatisfiedBy(obj);
             }

    }





    public class NotSpecification : CompositeSpecification
    {

         public NotSpecification(Specification leftSide, Specification rightSide): base(leftSide, rightSide)
         {

         }

         public override bool IsSatisfiedBy(T obj)
         {
              return _leftSide.IsSatisfiedBy(obj) && !_rightSide.IsSatisfiedBy(obj);
         }
    }   



and some methods in our base class as follows


    public abstract class Specification
    {
           public abstract bool IsSatisfiedBy(T obj);
           public AndSpecification And(Specification specification)
           {
                return new AndSpecification(this, specification);
           }

           public OrSpecification Or(Specification specification)
           {
                  return new OrSpecification(this, specification);
           }

           public NotSpecification Not(Specification specification)
           {
                return new NotSpecification(this, specification);
           }
    }




now we are ready to use some composite specifications as shown below

    public List GetExpensiveProductsInStock()
    {

          List products = GetAllProducts();
          List expensiveProducts = new List();
          foreach (product p in products)
          {

              if(new ProductInStockSpecification().And(new ProductPriceGreaterThanSpecification()).IsSatisfiedBy(p);)
              {
                   expensiveProducts.add(p)
              }
          }
          return expensiveProducts;
    }
