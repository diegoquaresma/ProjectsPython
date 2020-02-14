import logging

import azure.functions as func

from facebookads.adobjects.targetingsearch import TargetingSearch

from facebookads.api import FacebookAdsApi

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    token = 'EAAatqMY9YeQBAKo2WiunzO8xaxA0YwqQXfflxCLKuZAZA75i44dndFAYrXP1wwiaDnNElMPy28HalSgz52vI8qkSyW45u8C96o8MZA0hPQU8SA9ZCOhFKoySma4HZAtMR7JLZB262BEY6jDMIEPaKe2P4tTIsdllZBsrmWPyJjCh3of5Tl1hXRZBMG8kZBtM6ViNapBVpbrHw0tSqVPcwe9a9'
    FacebookAdsApi.init(access_token=token)

    params = {
        'q': name,
        'limit': 10000000,
        'locate': 'pt_br',
        'type': 'adinterest',
    }

    name = TargetingSearch.search(params=params)

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
