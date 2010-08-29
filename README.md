# uriparser2

Your one-stop C libray for URI parsing.

## Why?

There aren't any easy to use libraries right now. The original uriparser[1] works okay but its API is horrendous.

[1] http://uriparser.sourceforge.net/ - uriparser2 uses bits of uriparser but is not affiliated with or endorsed by said library.

## Compiling

Build the dynamic and static library:

	make all

## Usage

From `uriparser2.h`:

	/**
	 * URI object. After the call to uri_parse() fields will be NULL (0 for the port) if their component was absent in the input string.
	 */
	typedef struct {
		const char *scheme;
		const char *user;
		const char *pass;
		const char *host;
		unsigned short port;
		const char *path;
		const char *query;
		const char *fragment;
	} URI;

	/**
	 * Parse URI into its components.
	 *
	 * @param uri The URI to parse.
	 * @return URI object. The caller is responsible for freeing this object. NULL is returned on parse error or out-of-memory conditions (in the latter case errno=ENOMEM).
	 */
	URI *uri_parse(const char *uri);

	/**
	 * Create string representation of URI object.
	 *
	 * @param uri URI object.
	 * @return URI as a string. The caller is responsible for freeing this object. NULL is returned on out-of-memory conditions (errno=ENOMEM).
	 */
	char *uri_build(const URI *uri);

	/**
	 * Compare two URI objects. Follows the strcmp() contract. The order in which components are compared is as follows: scheme, host, port, path, query, fragment, user, pass.
	 * NULL components are always smaller than their non-NULL counterparts. That is, a < b if a->scheme == NULL and b->scheme != NULL.
	 *
	 * @param a First URI object.
	 * @param b Second URI object.
	 * @return -1 if a < b, 0 if a == b, 1 if a > b.
	 */
	int uri_compare(const URI *a, const URI *b);


## License

uriparser2 is MIT-licensed. The bits from the original uriparser - http://uriparser.sourceforge.net/ - are BSD-licensed.
