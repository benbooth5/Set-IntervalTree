[![Build Status](https://travis-ci.org/benwbooth/Set-IntervalTree.svg?branch=master)](https://travis-ci.org/benwbooth/Set-IntervalTree)
[![AppVeyor Status](https://ci.appveyor.com/api/projects/status/github/benwbooth/Set-IntervalTree?branch=master&svg=true)](https://ci.appveyor.com/project/benwbooth/Set-IntervalTree)

# NAME

Set::IntervalTree - Perform range-based lookups on sets of ranges

# VERSION

version 0.11\_01

# SYNOPSIS

    use Set::IntervalTree;
    my $tree = Set::IntervalTree->new;
    $tree->insert("ID1",100,200);
    $tree->insert(2,50,100);
    $tree->insert({id=>3},520,700);
    $tree->insert($some_obj,1000,1100);

    my $results = $tree->fetch(400,800);
    my $window = $tree->fetch_window(100,200);
    print scalar(@$results)." intervals found.\n";

    # remove only items overlapping location 100..200 with values 
    # less than 100;
    my $removed = $tree->remove(100,200 sub {
      my ($item, $low, $high) = @_;
      return $item < 100;
    });

# DESCRIPTION

Set::IntervalTree uses Interval Trees to store and efficiently 
look up ranges using a range-based lookup.

All intervals are half-open, i.e. \[1,3), \[2,6), etc.

# EXPORTS

Nothing.

# METHODS

my $tree = Set::IntervalTree->new;

    Creates a new interval tree object.

$tree->insert($object, $low, $high);

    Insert a range into the interval tree and associate it with a 
    perl scalar.

    $object can be any perl scalar. This is what will be returned by fetch().
    $low is the lower bound of the range.
    $high is the upper bound of the range.

    Ranges are represented as half-closed integer intervals.

my $results = $tree->fetch($low, $high)

    Return an arrayref of perl objects whose ranges overlap 
    the specified range.

    $low is the lower bound of the region to query.
    $high is the upper bound of the region to query.

my $results = $tree->fetch\_window($low, $high)

    Return an arrayref of perl objects whose ranges are completely contained
    witin the specified range.

    $low is the lower bound of the region to query.
    $high is the upper bound of the region to query.

my $nearest\_up = $tree->fetch\_nearest\_up($query)

    Search for the closest interval in upstream that does not contain the query
    and returns the perl object associated with it.

    $query is the position to use for the search

my $nearest\_down = $tree->fetch\_nearest\_down($query)

    Search for the closest interval in downstream that does not contain the query
    and returns the perl object associated with it.

    $query is the position to use for the search

my $removed = $tree->remove($low, $high \[, optional \\&coderef\]);

    Remove items in the tree that overlap the region from $low to $high. 
    A coderef can be passed in as an optional third argument for filtering
    what is removed. The coderef receives the stored item, the low point,
    and the high point as its arguments. If the result value of the coderef
    is true, the item is removed, otherwise the item remains in the tree.

    Returns the list of removed items.

my $removed = $tree->remove\_window($low, $high \[, optional \\&coderef\]);

    Remove items in the tree that are contained within the region from $low
    to $high.  A coderef can be passed in as an optional third argument
    for filtering what is removed. The coderef receives the stored item,
    the low point, and the high point as its arguments. If the result
    value of the coderef is true, the item is removed, otherwise the item
    remains in the tree.

    Returns the list of removed items.

# LIMITATIONS

A $tree->print() serialization method might be useful for debugging.

# SEE ALSO

The source code for this module contains a reusable template-based 
C++ header for Interval trees that might be useful.

# AUTHORS

- Benjamin Booth <benbooth@cpan.org>
- Stephan Loyd <sloyd@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2012 by Benjamin Booth.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
